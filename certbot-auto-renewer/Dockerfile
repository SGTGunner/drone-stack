FROM certbot/certbot
# Clear default certbot ENTRYPOINT
ENTRYPOINT [""]
# Try renewing certificates and then sleep for 12 hours before the container stops and restarts
CMD certbot renew && echo "sleeping for 12 hours" && sleep 43200 && echo "done sleeping"
