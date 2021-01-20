# OpenShift Custom Banner

Example of how to modify login page for a custom message or image

Generate Templates if needed
```
 oc create secret generic login-template --from-file=login.html -n openshift-config
 oc create secret generic providers-template --from-file=providers.html -n openshift-config
 oc create secret generic error-template --from-file=errors.html -n openshift-config

```
Modify the files to include banner then set them as secret

```
oc create secret generic login-template --from-file=login.html -n openshift-config
```

Update Oauth spec to point at html secrets

```
oc edit oauths cluster


-------- 
spec:
  templates:
    error:
        name: error-template
    login:
        name: login-template
    providerSelection:
        name: providers-template
```



Delete and Recreate
```
oc delete secret login-template -n openshift-config
oc create secret generic login-template --from-file=login.html -n openshift-config


```


**Notes**

-    If another file name is specified it will not work i.e.  mylogin.html
-    Images will need to be hosted from a place where a uri can reach them. Tested that Box does not work
     Consider possibly running an apache container to host the images if no other place can be found.
     base64 encodings can also be rendered as such `img src="data:image/svg+xml;base64`  see og-login.html
-    If specified and the secret or expected key is
     not found, the default provider selection page is used. If the specified
     template is not valid, the default provider selection page is used. If
     unspecified, the default provider selection page is used. The namespace for
     this secret is openshift-config.

More Info at 
https://github.com/ocp4opsandsecurity/OpenShift-Custom-Banner.git



