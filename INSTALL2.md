Create a systemd file which will allow you to reference the same service running on different ports

`vi /etc/systemd/system/samplicator@.service`


	[Unit]
	Description=Samplicator
	After=network.target

	[Service]
	Type=forking

	#SNMP Traps
	ExecStart=/usr/local/bin/samplicate -S -c /opt/samplicator/etc/samplicator%I.conf -p %I -d 0 -f

	[Install]
	WantedBy=multi-user.target


Create a configuraiton file for each port you want to listen on

`vi /opt/samplicator/etc/samplicator2055.conf`


	#Send any flow to SolarWinds
	0.0.0.0/0.0.0.0: 10.5.1.10/2055

	#Send edge router flows to Noction
	88.88.22.1/255.255.255.255: 99.99.22.22/2055
	88.88.22.2/255.255.255.255: 99.99.22.22/2055


`vi /opt/samplicator/etc/samplicator162.conf`


	#Send any flow to SolarWinds
	0.0.0.0/0.0.0.0: 10.5.1.10/2055


Create a script to help you start, stop, and restart services

`vi /usr/bin/samplicator`


	#!/bin/bash
	
	ports=(
	  162
	  2055
	  2056
	  4739
	  6343
	  9995
	  9996
	)
	
	for arg in "$@"
	do
	  if [ "$arg" == "--help" ] || [ "$arg" == "-h" ]
	  then
	    echo "--start    Start the Services"
	    echo "--stop     Stop the Services"
	    echo "--restart  Restart the Services"
	  elif [ "$arg" == "--start" ]
	  then
	    i=0
	    while (( i<${#ports[*]} )); do
	      echo "Starting samplicator on port ${ports[$i]}"
	      systemctl start samplicator@${ports[$i]}.service
	      ((i++))
	    done
	  elif [ "$arg" == "--stop" ]
	  then
	    i=0
	    while (( i<${#ports[*]} )); do
	      echo "Stopping samplicator on port ${ports[$i]}"
	      systemctl stop samplicator@${ports[$i]}.service
	      ((i++))
	    done
	  elif [ "$arg" == "--restart" ]
	  then
	    i=0
	    while (( i<${#ports[*]} )); do
	      echo "Restarting samplicator on port ${ports[$i]}"
	      systemctl restart samplicator@${ports[$i]}.service
	      ((i++))
	    done
	fi
	done


`samplicator --help`

`samplicator --start`

`samplicator --stop`

`samplicator --restart`
