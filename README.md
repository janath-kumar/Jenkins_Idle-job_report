This script will gives a report of idle job for long period of time in jenkins, it gives as a report as html format.

after build this need to approve for the following scripts in Jenkins ---> Manage jenkins ---> script Approval.
**Following scripts to approval**
    method hudson.model.Item getFullName
    method hudson.model.ItemGroup getAllItems java.lang.Class
    method hudson.model.Job getLastBuild
    method hudson.model.Run getTimeInMillis
    method jenkins.model.Jenkins getRootUrl
    staticMethod jenkins.model.Jenkins getInstanceOrNull
    staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods round java.lang.Double
    staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods toDouble java.lang.Number
