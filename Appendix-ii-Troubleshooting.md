# Appendix II - Troubleshooting

Here are a few issues and error messages I ran into while setting up the RockPro64 Simple Server and how I resolved them.

## "404 Page not found"
- Is the Docker container not up?

## "Gateway Timeout"
- fail2ban might be causing the problem. Check the TZ (timezone) variable and make sure they all match.
- Or just wait a bit for everything to sync?

## Duplicati: "Failed to process path"
- Turned out to be a permissions thing (the file and data folder were owned by "root" user. I gave my user ownership and that solved it.

## Apps suddenly break after installing a new program
- Check Traefik logs and read error messages: `sudo docker logs traefik`
- E.g., I installed Flame, then Gotify stopped working. Turns out it's because I hadn't changed the "router" name in the "Labels" section of the .yml file--I still had it written as "gotify" so I changed it to "flame" and that fixed it.
