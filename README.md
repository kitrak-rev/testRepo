
Encountered same issue. tibjms has jakarta compatible library that comes with Tibco EMS 10.2. Unfortunately, this library is not published in public maven repo. To be able to use this, download the latest Tibco EMS community edition, install in the system and you will be able to see the lib under tibco/ems/10.2/lib folder with name jakarta.jms-tibjms.jar Upload it to your private nexus repo and thats it. You need corresponding jakarta.jms-api dependency. This library is available in public maven repo.

https://stackoverflow.com/questions/75226702/tibcojmsconnectionfactory-configuration-issues
