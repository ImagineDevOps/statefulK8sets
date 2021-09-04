
    </div>
  </div>

  <div class="container" style="--line-height: 1.6em;" dir="ltr">
    <div class="header reader-header reader-show-element">
      <a class="domain reader-domain" href="https://www.howtoforge.com/create-a-statefulset-in-kubernetes/">howtoforge.com</a>
      <div class="domain-border"></div>
      <h1 class="reader-title">How to create a StatefulSet in Kubernetes</h1>
      <div class="credits reader-credits">Rahul Shivalkar</div>
      <div class="meta-data">
        <div class="reader-estimated-time" dir="ltr">4-5 minutes</div>
      </div>
    </div>

    <hr>

    <div class="content">
      <div class="moz-reader-content reader-show-element"><div id="readability-page-1" class="page">

<header>
<div id="header">
<div id="logo"><a href="https://www.howtoforge.com/">
<span></span>
<img src="StatefulSets-in-Kubernetes_files/howtoforge_logo_trans.gif" alt="Howtoforge" data-ezsrc="/images/howtoforge_logo_trans.gif">
</a></div>
<span></span>
</div>
</header>
<div id="content"><span id="ezoic-pub-ad-placeholder-105"></span>
<div>

<div id="htfContentPage">

<article>
<h2>How to create a StatefulSet in Kubernetes</h2>

<p>StatefulSets contain a set of Pods with unique, persistent identities
 and stable hostnames. A pod template is used in a Statefulset, which 
contains a specification for its pods, pods are created using this 
specification. We can deploy stateful applications and clustered 
applications using Statefulsets in Kubernetes. StatefulSet can be 
updated by making changes to its Pod specification, which includes its 
container images and volumes.&nbsp;&nbsp;<span id="ezoic-pub-ad-placeholder-106"></span><span data-ez-name="howtoforge_com-box-3"><span id="div-gpt-ad-howtoforge_com-box-3-0"></span></span></p>
<p>StatefulSets can be used when the applications require any of the following properties.<span id="ezoic-pub-ad-placeholder-121"></span><span data-ez-name="howtoforge_com-medrectangle-3"><span id="div-gpt-ad-howtoforge_com-medrectangle-3-0"></span></span>
</p><ul>
<li>Stable, unique network identifiers.
</li><li>Stable, persistent storage.
</li><li>Ordered, graceful deployment and scaling.
</li><li>Ordered, automated rolling updates.
</li></ul>
<p>For a StatefulSet with N replicas, when Pods are being deployed, they
 are created sequentially, in order from {0..N-1}.&nbsp;When Pods are 
being deleted, they are terminated in reverse order, from {N-1..0}.
</p><p>To know more about Statefulset, click <a href="https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/">here</a>.
</p><p>In this article, we will create a Statefulset with replicas of 
Nginx pods. We will perform operations on the Pods to see how they are 
deleted and created.
</p><h2 id="prerequisites">Pre-requisites</h2>
<ol>
<li>Kubernetes Cluster with at least 1 worker node.<br>If you want to learn to create a Kubernetes Cluster, click&nbsp;<a href="https://www.howtoforge.com/setup-a-kubernetes-cluster-on-aws-ec2-instance-ubuntu-using-kubeadm/">here</a>. This guide will help you create a Kubernetes cluster with 1 Master and 2 Nodes on AWS Ubuntu 18l04 EC2 Instances.&nbsp;
</li></ol>
<h2 id="what-we-will-do">What we will do</h2>
<ol>
<li>Create a&nbsp;Statefulset
</li></ol>
<h2 id="create-anbspstatefulset">Create a&nbsp;Statefulset</h2>
<p>Create a file and add the following Statefulset definition in it.<span id="ezoic-pub-ad-placeholder-130"></span><span data-ez-name="howtoforge_com-link-h-large-1"></span></p>
<p>vim statefulset.yml
</p><pre>apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web</pre>
<p><a id="img-screenshot_2020-06-21_at_112541_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_112541_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_112541_am.webp" alt="statefulset" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_112541_am.png?ezimgfmt=rs:750x750/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="750"></a>
</p><p>In this example,&nbsp;
</p><ul>
<li>A Headless Service, named&nbsp;<code>nginx</code>, is used to control the network.
</li><li>The StatefulSet, named web, has 3 replicas of the nginx container that will be launched in unique Pods.
</li><li>nginx Image with version slim:0.8 is used to deploy Nginx.
</li></ul>
<p>To create a Statefulset, execute the following commands.
</p><p>kubectl get statefulset
</p><p>kubectl create -f statefulset.yml
</p><p><a id="img-screenshot_2020-06-21_at_112734_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_112734_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_112734_am.webp" alt="create-stateful-set" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_112734_am.png?ezimgfmt=rs:750x198/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="198"></a>
</p><p>Execute the following 2 commands to list the Statefulset and Service created in the above step.<span id="ezoic-pub-ad-placeholder-110"></span>
</p><p>kubectl get statefulset
</p><p>kubectl get service
</p><p><a id="img-screenshot_2020-06-21_at_112828_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_112828_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_112828_am.webp" alt="get-statefulset-and-service" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_112828_am.png?ezimgfmt=rs:750x194/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="194"></a>
</p><p>Get the pods using the following command and see the Pods have numbers as Suffix in the Pod name.&nbsp;
</p><p>kubectl get pods<span id="ezoic-pub-ad-placeholder-111"></span>
</p><p><a id="img-screenshot_2020-06-21_at_112949_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_112949_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_112949_am.webp" alt="get-pods-in-statefulset" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_112949_am.png?ezimgfmt=rs:750x173/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="173"></a>
</p><p>To get the complete details of the Statefulset, execute the following commands.
</p><p>kubectl get statefulset
</p><p>kubectl describe statefulset web<span id="ezoic-pub-ad-placeholder-112"></span>
</p><p><a id="img-screenshot_2020-06-21_at_113037_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_113037_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_113037_am.webp" alt="describe-statefu-set" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_113037_am.png?ezimgfmt=rs:750x407/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="407"></a>
</p><p>Now, let's delete the pods and see how names are preserved even after new pods are created.
</p><p>We are deleting 2 pods to see what names will be assigned to the new pods upon creation.
</p><p>kubectl get pods
</p><p>kubectl delete pods web-0 web-2
</p><p><a id="img-screenshot_2020-06-21_at_113200_am" href="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/big/screenshot_2020-06-21_at_113200_am.png"><img src="StatefulSets-in-Kubernetes_files/screenshot_2020-06-21_at_113200_am.webp" alt="recreate-pods" data-ezsrc="https://www.howtoforge.com/images/create_a_statefulset_in_kubernetes/screenshot_2020-06-21_at_113200_am.png?ezimgfmt=rs:750x402/rscb5/ng:webp/ngcb5" moz-reader-center="true" width="750" height="402"></a>
</p><p>In the above screenshot you can see that, even after deleting the pods, newly created pods get the same name.
</p><h2 id="conclusion">Conclusion</h2>
<p>In this article, we created a Statefulset and performed operations on
 it to check its details. We also deleted the pods to see how the name 
of the pod is preserved and the same is assigned to the newly created 
pods after deleting it.
</p></article>


</div>
<div id="htfContentPagePaging">
<h2>Suggested articles</h2>
</div>
<div id="htfContentPageComment"><span id="ezoic-pub-ad-placeholder-118"></span>
<a name="comments"></a>
<h2>0 Comment(s)</h2>

</div>




</div>
</div>


<div id="subscriptionModal">
<span>×</span>
<p>This feature is only available to subscribers. Get your subscription <a href="https://www.howtoforge.com/subscription">here</a>.
</p></div>











</div></div>
    </div>

    <div>
      <div class="reader-message"></div>
    </div>
    <div aria-owns="toolbar"></div>
  </div>



</body></html>
