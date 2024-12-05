+++
title = "Redis Keys Count Bash"
date = 2024-10-09
type = "til"
description = "Description about Redis Keys Count Bash"
in_search_index = true
[taxonomies]
tags = ["redis","tech"]
+++

Wanted to fetch a script to find keys matching pattern from redis, without blocking the redis server.

Created a bash snippet for the same use case.

Assumptions..

1. Redis doesn't use any password
2. `redis-cli` is already installed

Usage

1. Save the script as `find_redis_keys.sh`
2. Make the script executable by `chmod +x` `find_redis_keys.sh`
3. Run the script using `./list_redis_keys_with_count.sh "user:*"`

```bash
#!/bin/bash

# Declare Redis connection constants
REDIS_HOST="127.0.0.1"
REDIS_PORT="6379"

# Check if a pattern argument is provided
if [ -z "$1" ]; then
  echo "Usage: $0 <pattern>"
  exit 1
fi

# Define the pattern passed as an argument
PATTERN=$1

# Initialize cursor to 0 for the first scan
CURSOR=0

# Loop until the cursor is 0 (indicating the scan is complete)
while true; do
  # Run the scan command and capture the result
  RESULT=$(redis-cli -h "$REDIS_HOST" -p "$REDIS_PORT" --scan --pattern "$PATTERN" --cursor "$CURSOR")

  # Extract the new cursor and the keys from the result
  CURSOR=$(echo "$RESULT" | head -n 1)
  KEYS=$(echo "$RESULT" | tail -n +2)

  # Print the keys
  echo "$KEYS"

  # If the cursor is 0, we've finished scanning
  if [ "$CURSOR" == "0" ]; then
    break
  fi
done

```
