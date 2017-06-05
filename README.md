# Clearwater Heat Templates

The default clearwater project fails to allocate IP address to IMS nodes.


I decided to modify the network implementation of heat templates to make things work.


With these modifications, users can create their own private/project network and then set the name into input parametes.

Main modifications:

- add parameter project_net in clearwater.yaml and in bono.yaml, dns.yaml, ellis.yaml, homer.yaml, homestead.yaml, ralf.yaml, sprout.yaml

- modify the way to get floating ip for server resource

            floating_ip:
              type: OS::Neutron::FloatingIP
              properties:
                floating_network_id: { get_param: public_net_id }
			  
          ....
          server:
            type: OS::Nova::Server
            properties:
              name: { str_replace: { params: { __zone__: { get_param: zone } }, template: ns.__zone__ } }
              image: { get_param: image }
              flavor: { get_param: flavor }
              key_name: { get_param: key_name }
              networks:
                - network: { get_param: project_net }
            ....
			
		  association:
            type: OS::Neutron::FloatingIPAssociation
            properties:
              floatingip_id: { get_resource: floating_ip }
              port_id: {get_attr: [server, addresses, {get_param: project_net}, 0, port]}
