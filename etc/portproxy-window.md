netsh interface portproxy add v4tov4 listenport=19001 listenaddress=172.10.0.1 connectport=19001 connectaddress=172.12.0.1

netsh interface portproxy show v4tov4