### Docker Node Commands

Docker node commands manage Swarm nodes


Command 		 	    Description
docker node demote 	    Demote one or more nodes from manager in the swarm
docker node inspect     Display detailed information on one or more nodes
docker node ls 		    List nodes in the swarm
docker node promote     Promote one or more nodes to manager in the swarm
docker node ps 		    List tasks running on one or more nodes, defaults to current node
docker node rm 		    Remove one or more nodes from the swarm
docker node update 	    Update a node

~~~~
# docker node ls
ID                           HOSTNAME                            STATUS  AVAILABILITY  MANAGER STATUS
hygin6n0xmv912t9dkeio0qtr   test03  Ready   Drain
rz8agwwle20z2p8pffr79dhbk    test04  Down    Active
~~~~


docker node update
Update the status of a node

docker node update [OPTIONS] NODE

Options
Name, shorthand 	Default 	Description
--availability		Availability of the node (“active”|“pause”|“drain”)
--label-add		    Add or update a node label (key=value)
--label-rm		    Remove a node label if exists
--role			    Role of the node (“worker”|“manager”)

~~~~
docker node update --availability="drain" test02 
seay8vqcontg6nrkm7ilaaczo    test02  Down    Drain
wxhkcgqhi9bfld22loknhbo0m *  test01  Ready   Active        Leader
yvww4k0goui3gv61dpt3vipat    test05  Ready   Active
~~~~