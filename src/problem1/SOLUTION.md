Provide your CLI command here:

grep "TSLA" transaction-log.txt | jq -r '.order_id' | xargs -I {} curl -s "https://example.com/api/{} >> output.txt

