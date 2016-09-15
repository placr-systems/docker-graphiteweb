# Usage

This docker provides access to the graphite web part of the graphite stack. It does not include the carbon/whisper part.

It is the intent to run this together with the placr/carbon, placr/statsd and grafana dockers. Basically only the API part of the graphiteweb Django application is used. No users are created on graphite web and no volume is mapped for the database storage.

Example, first start the carbon docker (see also placr/carbon):

	docker run -d -v $PWD/whisper/:/opt/graphite/storage/whisper/ --name carbon placr/carbon
	
Then start the graphiteweb docker:

	docker run -d --name graphiteweb --link carbon --volumes-from carbon -p 80:80 placr/graphiteweb

If you want NGINX to front graphite web, start the graphiteweb docker without the port mapping and start the standard nginx docker with the volumes from this docker:

	docker run -d --name nginx --volumes-from graphiteweb -p 8811:80 nginx
	