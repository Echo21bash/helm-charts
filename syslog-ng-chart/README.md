# syslog-ng-chart
This chart will deploy an basic syslog-ng server using helm. Nothing special so far, but maybe it can help someone else :-)

# Deployment
To deploy the chart, you can just use helm. `helm install syslog-ng` should do the trick. Via `helm template syslog-ng --dry-run` you can have a look at the generated k8s manifests.

Enjoy!
