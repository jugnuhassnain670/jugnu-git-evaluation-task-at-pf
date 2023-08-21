Step 01: Creating a GitHub repository & setting GitHub remote repository to local
step 02: Create a simple app.js file for show any message.
step 03: Create a feature branch & change some code in app.js and create a pull request.
step 04: Create a pre-commit file in .git/hooks and write a bash script in it that will check code 
         before commiting it with following rules.
            1) No trailing whitespace allowed.
            2) No debugging statements (e.g., console.log) allowed.

            Script code is this
            #!/bin/bash

            # Prevent commits with trailing whitespace
            if git diff --check --cached | grep -E '\s+$'; then
            echo "Error: Commit contains trailing whitespace."
            exit 1
            fi

            # Prevent commits with debugging statements
            if git diff --cached | grep -E '^\+' | grep -E 'console\.log'; then
            echo "Error: Commit contains debugging statements."
            exit 1
            fi

step 05: Create a post-commit file in .git/hooks and write a bash script that send a notification in whistleit channel.....
            #!/bin/bash

            # Replace these placeholders with actual values
            WEBHOOK_URL="https://app.whistleit.io/api/webhooks/64d1f8fc602e1e5d893bc831"

            # Get the last commit message
            COMMIT_MESSAGE=$(git log -1 --pretty=format:%s)

            # Check if the commit message contains "Merge pull request"
            if [[ $COMMIT_MESSAGE == *"Merge pull request"* ]]; then
                # Craft the message you want to send to Slack
                MESSAGE="  Congrats on merging a pull request: '$COMMIT_MESSAGE'!"
                curl -X POST -d "message=$MESSAGE" "$WEBHOOK_URL"

                # Send the message to Slack
                #curl -X POST -H 'Content-type: application/json' --data "{'text':'$MESSAGE'}" $SLACK_WEBHOOK_URL
            fi
