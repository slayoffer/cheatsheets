What commands did you use to perform the following?

1. Create a deployment running one pod using the official NGINX image.

2. Expose that deployment.

3. Check that you can successfully connect to the exposed service.

k create deploy nginx --image nginx

k expose deploy nginx --port 80

kubectl run --restart=Never --image=alpine -ti --rm testcontainer

apk add curl

curl nginx

What commands did you use to perform the following?

1. Change (edit) the service definition to use a label/value of _myapp: web_

2. Check that you _cannot_ connect to the exposed service anymore.

3. Change (edit) the deployment definition to add that label/value to the pod template.

4. Check that you *can* connect to the exposed service again.

k edit svc nginx

kubectl run --restart=Never --image=alpine -ti --rm testcontainer

apk add curl

curl nginx

k edit deploy nginx

kubectl run --restart=Never --image=alpine -ti --rm testcontainer

apk add curl

curl nginx

What commands did you use to perform the following?

1. Create a deployment running one pod using the official Apache image (httpd).

2. Change (edit) the deployment definition to add the label/value picked previously.

3. Connect to the exposed service again.

(It should now yield responses from both Apache and NGINX.)

k create deploy apache --image httpd

k edit deploy apache

kubectl run --restart=Never --image=alpine -ti --rm testcontainer

apk add curl

curl nginx