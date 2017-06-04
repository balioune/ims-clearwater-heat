# Clearwater Heat Templates

The default clearwater project fails to allocate IP address to IMS nodes.


I decided to modify the network implementation of heat templates to make things work.


With these modifications, users can create their own private/project network and then set the name into input parametes.