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
