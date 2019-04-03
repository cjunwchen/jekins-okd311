# 1. Install Jenkins
Install Jenkins is straight forward, just google it and follow the instruction

# 2. Create Service Account in OpenShift
Login into OpenShift cluster

	oc login
	#login with admin user name and password

Create a new project, such as "ci"	
	
	oc new-project ci

Create service account

	oc create serviceaccount jenkins
	oc policy add-role-to-user edit system:serviceaccount:ci:jenkins -n <project>
	oc adm policy add-cluster-role-to-user edit system:serviceaccount:ci:jenkins

# 3. Optain access token for service account
	
	oc serviceaccounts get-token jenkins -n ci
	#Copy the returned value as it will be needed in the following sections

# 4. Config credential in Jenkins server

Login into Jenkins, click **"Credentials"**, next to **"Jenkins"** within the **"Stores scoped to Jenkins"**, select **"(global)"**, and click on the *left-hand* side, select **"Add Credentials"**. Copy the token obtained above and paste it in "Token" below.

![](https://github.com/cjunwchen/jekins-okd311/blob/master/images/jenkins-okd311.png)
 
# 5. Install OpenShift plugin in Jenkins

Click **"Manage Jenkins**, and then click **"Manage Plugin"**, install **"OpenShift Client Plugin"**

# 6. Configure OpenShift Client Plugin

Click **"Manage Jenkins"**, and then click **"Configure System"**, jump to **"OpenShift Client Plugin"**

![](https://github.com/cjunwchen/jekins-okd311/blob/master/images/jenkins-okd-plugin.png)
- Cluster name: put a name
- API Server URL: this is url for OpenShift management url
- Credientials: the crediential created above (step 4)

# 7. Install oc binary client in Jenkins server
Login into Jenkins server, download the oc client from [Openshift github link](https://github.com/openshift/origin/releases/),  extract the tar file and copy the oc binary file into /usr/bin directory

# 8. Test out

Create a pipeline, use the pipeline script below

	openshift.withCluster( 'okd-master1' ) {
		stage "get info…."   
			/** Selectors are a core concept in the DSL. They allow the user to invoke operations **/
			/** on group of objects which satisfy a given criteria. **/
	
			// Create a Selector capable of selecting all service accounts in mycluster's default project
			def saSelector = openshift.selector( 'serviceaccount' )
			
			// Prints `oc describe serviceaccount` to Jenkins console
			saSelector.describe()
	
			// Selectors also allow you to easily iterate through all objects they currently select.
			saSelector.withEach { // The closure body will be executed once for each selected object.
				// The 'it' variable will be bound to a Selector which selects a single
        			// object which is the focus of the iteration.
        			echo "Service account: ${it.name()} is defined in ${openshift.project()}"
			}
	
			// Prints a list of current service accounts to the console
			echo "There are ${saSelector.count()} service accounts in project ${openshift.project()}"
			echo "They are named: ${saSelector.names()}"
		
		stage "building…."   
			echo "building…."
	   
		stage "deploying…."   
			echo "deploying…."
	}

Trigger the pipeline by click "Build Now". The sample [Jenkinsfile](https://github.com/cjunwchen/jekins-okd311/blob/master/Jenkinsfile) here is for creating a new app.

# 9. Reference
[OpenShift Jenkins Client Plugin](https://github.com/openshift/jenkins-client-plugin/blob/master/README.md#setting-up-credentials)





  

