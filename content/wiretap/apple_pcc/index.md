+++
title = "Apple Private Cloud Compute: First Glance"
date = 2024-06-20
[taxonomies]
tags = ["apple", "security", "PCC"]
+++

[PCC: Bold step forward, not without flaws](https://blog.trailofbits.com/2024/06/14/pcc-bold-step-forward-not-without-flaws/)

[Private Cloud Compute: A new frontier for AI privacy in the cloud](https://security.apple.com/blog/private-cloud-compute/)

It's incredible how much effort that goes into a designing a solution like this from scratch and have security in mind. It's not an easy feat but let's take a look at some positives and negatives:

> Private Cloud Compute hardware security starts at manufacturing, where we inventory and perform high-resolution imaging of the components of the PCC node before each server is sealed and its tamper switch is activated. When they arrive in the data center, we perform extensive revalidation before the servers are allowed to be provisioned for PCC.

Securing the supply chain ✅

> We supplement the built-in protections of Apple silicon with a hardened supply chain for PCC hardware, so that performing a hardware attack at scale would be both prohibitively expensive and likely to be discovered. We limit the impact of small-scale attacks by ensuring that they cannot be used to target the data of a specific user.

Further securing the supply chain from attacks by making it crazy expensive if attempted at scale.

> The root of trust for Private Cloud Compute is our compute node: custom-built server hardware that brings the power and security of Apple silicon to the data center, with the same hardware security technologies used in iPhone, including the Secure Enclave and Secure Boot.

Baking in their existing hardware security technologies to hold keys and protection features. What about the ingress/egress traffic?

> Target diffusion starts with the request metadata, which leaves out any personally identifiable information about the source device or user, and includes only limited contextual data about the request that’s required to enable routing to the appropriate model. This metadata is the only part of the user’s request that is available to load balancers and other data center components running outside of the PCC trust boundary. The metadata also includes a single-use credential, based on RSA Blind Signatures, to authorize valid requests without tying them to a specific user. Additionally, PCC requests go through an OHTTP relay — operated by a third party — which hides the device’s source IP address before the request ever reaches the PCC infrastructure.

"Target diffusion", I like the terminology. It's a fancy way of saying we obfuscate your data and make sure it's secretative while it's in transit both ways.

I think that mostly covers all the physical security concerns. Moving onto software side of things:

> Traditional cloud services do not make their full production software images available to researchers — and even if they did, there’s no general mechanism to allow researchers to verify that those software images match what’s actually running in the production environment. (Some specialized mechanisms exist, such as Intel SGX and AWS Nitro attestation.)

> When we launch Private Cloud Compute, we’ll take the extraordinary step of making software images of every production build of PCC publicly available for security research. This promise, too, is an enforceable guarantee: user devices will be willing to send data only to PCC nodes that can cryptographically attest to running publicly listed software.

> Every production Private Cloud Compute software image will be published for independent binary inspection — including the OS, applications, and all relevant executables, which researchers can verify against the measurements in the transparency log.

They can't say how much of the code will be released for security review but they claim said "extraordinary" and something that other cloud providers do not provide to an extent. This will be revisited at a later time once they give more details on how the attestation process works. Knowing Apple, I would be surprised if they were generous about how much "code" they're willing to give for public review and access. They are quite secretive and restrictve in general (like any other company I guess).

> Stateless computation on personal user data. Private Cloud Compute must use the personal user data that it receives exclusively for the purpose of fulfilling the user’s request. This data must never be available to anyone other than the user, not even to Apple staff, not even during active processing. And this data must not be retained, including via logging or for debugging, after the response is returned to the user. In other words, we want a strong form of stateless data processing where personal data leaves no trace in the PCC system.

There are several mentions of "stateless" computing and data processing which is great to protect data from being stored or used in places you don't want to, especially when it's sensitive data or even metadata. I like it already.

From an AI/ML security perspective, Trail of Bits provided technical insights into the security and privacy limitations of AI/ML workloads that Apple might be trying to solve. For instance hardware limitations with existing devices and the ecosystem that it lives in:

> The term “requires unencrypted access” is the key to the PCC design challenges. Apple could continue processing data on-device, but this means abiding by mobile hardware limitations. The complex ML workloads Apple wants to offload, like using Large Language Models (LLM), exceed what is practical for battery-powered mobile devices. Apple wants to move the compute to the cloud to provide these extended capabilities, but HE doesn’t currently scale to that level. Thus to provide these new capabilities of service presently, Apple requires access to unencrypted data.

This is interesting as it does show a possible glimpse of the direction we are going to be moving towards in the future of mobile. Rather than waiting for a solution to solving the hard problem of increasing battery life on mobile devices, it may eventually be a energy efficient slimmer screen along with a regular sized battery that will last for days without charge. All the computing will be performed at provider's Edge network and all your data will be stored on the cloud. It's not too far fetched to be moving towards this direction and we're already seeing signs of it. If we do go down this road, are we willing to trust your provider that they are going to respect your privacy? How would you verify it? Are you okay with being locked into a single vendor? Can we opt out of this edge computing network? Would this edge compute model operate differently in other countries? These are all hard questions to be answering for a vendor and as a consumer. Interesting times we live in.