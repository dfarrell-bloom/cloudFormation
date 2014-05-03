= Proof of Concept: Route53 CloudFormation Updates

CloudFormation can update Route53 to automatically manage DNS.

== Caveats
<ul>
<li><p><em>CloudFormation cannot create hosted zones at this time</em></em>
    <p>According to <a href='http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-route53-recordset.html'>The CloudFormation <code>AWS::Route53::RecordSet</code> doc</a>, the hosted zone must already be created in Route53.
    </p>
</li>
<li>
    <em>There is no Public DNS Address associated with a public IP in a VPC ( whether or not an EIP ).  </p>
</li>
<li>
    <em>A Public IP can only be assigned to a Network Interface if there is only one interface configuration embedded in the Inscance resource</em>
    <p>CloudFormation will say <q>The associatePublicIPAddress parameter cannot be specified when launching with multiple network interfaces</q></p>
    </li>

== Examples

* `r53.json` - cloud formation template to setup a VPC and start an instance with public and private IP, and then registers the IPs in a specified DNS domain.

* `r53-eip.json` - same template, but assigns an EIP and registers that EIP in DNS.  Can be applied after the first template ( example of how one could assign an EIP with a later cloud-formation run ).