#!/usr/bin/env bash

# Exit on missing variables and failed pipelines
set -eu

# The first argument is the store location
# Use realpath to normalize the path
store_location="$(realpath "$1")" || { \
  echo >&2 "Error: the first argument is missing or invalid"
  echo >&2 "Usage: $0 <passwords_directory>"
  exit 1;
}

# Do not edit this line, you will mess the order
echo "login_password,folder,favorite,type,name,notes,fields,reprompt,login_uri,login_username"

# Choose the find command
if command -v fd 2>&1 > /dev/null;
then find_command=(fd --glob --print0 '*.gpg' "$store_location")
else find_command=(find "$store_location" -name '*.gpg' -print0)
fi

# iterate over all the found passwords
"${find_command[@]}" | while IFS= read -r -d '' filepath; do
  # decrypt the password file and duplicate all quote (") characters.
  # in order to export all characters, including newlines, the fields
  # must be surrounded with quotes and any existent quotes must be escaped.
  # quotes are escaped in csv by duplicating them.
  content="$(gpg -d "${filepath}" 2>/dev/null | sed 's/\"/\"\"/g')";

  # The following code simply sets the value of every csv field
  # expected by bitwarden and sometimes edit the content to remove
  # the data that's already extracted, leaving the rest as just notes
  
  # Extract the password (csv: login_password)
  login_password="$(echo -e "$content" | head -1)";
  # Remove the password from the decrypted content
  content="$(echo -e "$content" | sed '1d')";
  # Store the folder path (csv: login_password)
  folder="$(dirname "$(realpath --relative-to="$store_location" "$filepath")")"
  # The type is always login (csv: type)
  type='login'
  # The name is the file name (csv: name)
  name="$(basename "$filepath" .gpg)"
  # Get the username from either the filename or the content of the file
  # Remove the line containing the username if it's found in the content
  regex='^user\(name\)\?:'
  login_username="$(echo -e "$content" | grep "$regex" | cut -d':' -f2 | sed 's/^ //')"
  if [[ -z "$login_username" ]];
  then login_username="$name";
  else content="$(echo -e "$content" | sed /"$regex"/d)"
  fi
  # Skip favorite, fields, reprompt, login_uri
  favorite=''
  fields=''
  reprompt=''
  login_uri=''
  # What's left from the deecrypted content is stored as notes
  notes="$content"

  # Do not edit this line, you will mess the order
  printf '"%s",' "$login_password" "$folder" "$favorite" "$type" "$name" "$notes" "$fields" "$reprompt" "$login_uri" 
  printf '"%s"\n' "$login_username"
done;
