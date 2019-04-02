# 1. Install Jenkins
Install Jenkins is straight forward, just google it and follow the instruction

# 2. Service Account Creation within OpenShift
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

# 4. Config credential in Jenkins

Login into Jenkins, click **Credentials**, next to **Jenkins** within the **Stores scoped to Jenkins**, select **(global)**, and click on the *left-hand* side, select **Add Credentials**. Copy the token obtained above and paste it in "Token" below.


 


  

