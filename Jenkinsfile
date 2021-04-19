// Powered by Infostretch 

timestamps {

node () {

	stage ('Arest_EVPN_1-checkSSHConnection2 - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'baltah666', url: 'https://github.com/baltah666/Aresta2_ESXI']]]) 
	}
	stage ('Arest_EVPN_1-checkSSHConnection2 - Build') {
 			// Shell build step
sh """ 
ansible all -m ping 
 """ 
	}
	stage ('Arest_EVPN_2-CheckArestaVersion2 - Build') {
 			// Shell build step
sh """ 
ansible spines -m eos_command -a "commands='show version'" 
ansible leafs -m eos_command -a "commands='show version'" 
 """ 
	}
	stage ('Arest_EVPN_3- FullyDeployEnvironment2 - Build') {
 			// Shell build step
sh """ 
ansible-playbook deploy_evpn.yml 
 """ 
	}
	stage ('Arest_EVPN_4-CheckEvpnDeploy2 - Build') {
 			// Shell build step
sh """ 
ansible spines -m eos_command -a "commands='show bgp evpn summary'"
ansible leafs -m eos_command -a "commands='show bgp evpn summary'" 
 """ 
	}
	stage ('Arest_EVPN_6-DeployingL2VXLANService2 - Build') {
 			// Shell build step
sh """ 
ansible-playbook deploy_l2vxlan.yml -l 'vtep1,vtep2' -e "vlan_name=ansible_test vlan_id=40" 
 """ 
	}
	stage ('Arest_EVPN_7-DeployTenantVRFLeafs2 - Build') {
 			// Shell build step
sh """ 
ansible-playbook deploy_vrf.yml -l leafs -e "vrf_name=ansible vrf_id=1" 
 """ 
	}
	stage ('Arest_EVPN_8-CheckVxlan-CheckVRF2 - Build') {
 			// Shell build step
sh """ 
ansible leafs -m eos_command -a "commands='show vlan'" 
echo "**********************************************************************************************************************************************************************************************************************************************************************************************"
echo "**********************************************************************************************************************************************************************************************************************************************************************************************"

ansible leafs -m eos_command -a "commands='show vxlan vni '"
echo "**********************************************************************************************************************************************************************************************************************************************************************************************"
echo "**********************************************************************************************************************************************************************************************************************************************************************************************"
ansible leafs -m eos_command -a "commands='show vrf '" 
 """ 
	}
}
}