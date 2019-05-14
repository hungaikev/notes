# Lightbend Servers

Last Edited: Aug 06, 2018 4:42 PM
Tags: Important,Lightbend

`HapDgskasUPBJrMhCopwvFZ9`

`conduct info | grep sherpa`

`ssh -i ConductR.pem ubuntu@goldengate.typesafe.com`


`cd sherpa/sherpa/sherpa-akka`


//git pw = HapDgskasUPBJrMhCopwvFZ9


`sbt bundle:dist`


`cd ~/2.0`


`conduct info | grep sherpa`


`conduct load <NEWZIP> sherpa-akka-prod-3674ac19c31cc3aebb81ad7ef977cb11499570af1b80fdef308e0b7e76b4d275.zip`


`conduct run NEWBUNDLE --scale 3`


`conduct stop EXISTINGBUNDLE`


//PROCESS BAD OPPTY
`ssh -i CS-Admin.pem [ubuntu@54.87.176.248](mailto:ubuntu@54.87.176.248) -L 5601:10.0.1.28:5601` for debugging
`localhost:5601`
