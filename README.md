# openshift-appdynamics-jboss-cartridge 

This OpenShift embedded/plugin cartridge will enable AppDynamics monitoring on JBoss AS, JBoss EAP, JBoss EWS applications. It will install AppDynamics AppServer Agent on the gear and will report metrics to the configured AppDynamics controller.

## Install ##

Once you have created a Jboss application (AS,EAP or EWS only) either through the OpenShift Web or RHC command line util, please follow the next step to add the jboss appDynamics cartridge. 



```
	rhc add-cartridge -a <app_name> \
				  -e APPDYNAMICS_CONTROLLER_HOST_NAME=<appd_contr_host> \
				  -e APPDYNAMICS_CONTROLLER_PORT=<appd_contr_port> \ 
				  -e APPDYNAMICS_AGENT_APPLICATION_NAME=<appd_application_name> \
				  -e APPDYNAMICS_AGENT_TIER_NAME=<appd_tier_name> \
				  -e APPDYNAMICS_CONTROLLER_SSL_ENABLED=<true/false>\
				  -c http://cartreflect-claytondev.rhcloud.com/github/Appdynamics/openshift-appdynamics-jboss-cartridge

```

## SAAS environment ## 

If it is a dedicated or a multi-tenant controller, please pass the accountName and accountKey as well to the above command. 

```
	rhc add-cartridge -a <app_name> \
				  -e APPDYNAMICS_CONTROLLER_HOST_NAME=<appd_contr_host> \
				  -e APPDYNAMICS_CONTROLLER_PORT=<appd_contr_port> \ 
				  -e APPDYNAMICS_AGENT_APPLICATION_NAME=<appd_application_name> \
				  -e APPDYNAMICS_AGENT_TIER_NAME=<appd_tier_name> \
				  -e APPDYNAMICS_CONTROLLER_SSL_ENABLED=<true/false>\
				  -e APPDYNAMICS_AGENT_ACCOUNT_NAME=<appd_agent_accountname>
				  -e APPDYNAMICS_AGENT_ACCOUNT_ACCESS_KEY=<appd_agent_account_access_key>
				  -c http://cartreflect-claytondev.rhcloud.com/github/Appdynamics/openshift-appdynamics-jboss-cartridge

```
Make sure you have the right ports opened for SSL  and non-ssl  configurations.


Please note that the AppDynamics NodeName will be picked up as the Gear UUID..


The application has to be restarted either by using the above restart command or by pushing code which would inherently restart the application. 



```
	rhc app restart <app_name>
```



## Remove ##

```
	rhc cartridge-remove appdynamics-jboss-cart -a <app_name>
```

## Versions Supported ##

AppDynamics AppServerAgent 3.8.4,3.9.6,4.1.5,4.1.6

## To generate the RPM ##

Use fpm utility
```
	git clone https://github.com/jordansissel/fpm
  	gem install fpm
```
Generate version 1.0-3 rpm using fpm

```
	fpm --iteration=3 -s dir -t rpm -n appdynamics-jboss-cart ~/openshift-appdynamics-jboss-cartridge
```

## Upgrade / Downgrade ##
To use a different version of AppDynamics agent (current default 4.1.6.0 to say, 4.1.5.0) make a fork of
the repo and change version in metadata/manifest.yml. Use the fork'd repo when adding the cartridge
