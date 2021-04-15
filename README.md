# buildtest

Repo for testing openshift build
~~~
$ oc new-project buildtest

$ cat << 'EOF' > bc.yaml
apiVersion: build.openshift.io/v1
kind: BuildConfig
metadata:
  name: copy-test
spec:
  source:
    type: Git
    git:
      uri: https://github.com/kofibreik/buildtest.git
      ref: main
    dockerfile: |
      FROM registry.access.redhat.com/ubi8/ubi:latest
      COPY sql/*.sql /testdir/sql/
      CMD ["echo", "Hello World"]
  strategy:
    type: Docker
  output:
    to:
      kind: ImageStreamTag
      name: 'sample-image:latest'
EOF

$ oc create is sample-image

$ oc start-build copy-test --build-loglevel=6 -F

$ oc run sleep --image=image-registry.openshift-image-registry.svc:5000/buildtest/sample-image -- tail -f /dev/null
~~~
