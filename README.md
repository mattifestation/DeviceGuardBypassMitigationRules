# DeviceGuardBypassMitigationRules
A reference Device Guard code integrity policy consisting of FilePublisher deny rules for published Device Guard configuration bypasses.

As new Device Guard configuration bypasses are published, this reference policy will be updated with deny rules for the offending binaries. Generally speaking, the rules that will be published here will reflect signed Microsoft user-mode binaries that circumvent user-mode code integrity (UMCI). All code integrity policies will require that Microsoft binaries be trusted to a great extent, therefore it is reasonable to assume that a binary that executes arbitrary, unsigned code is a valid device guard configuration bypass.

If you believe this is missing a published bypass, please file a GitHub issue linking to the published bypass. I also ask that you validate these rules on your system. I can only obtain so many versions of the bypass binaries so there may be a version out there that was signed with a different code signing certificate that I'm not tracking. If that's the case, please let me know, provide the binary, and I will promptly update the policy. Thank you!

You can use the following code snippet to easily merge this policy with your existing code integrity policy:

```posh
# The path to the denial policy from the GitHub repo
$DenialPolicyFilePath = 'BypassDenyPolicy.xml'

# Replace this with the file path of the policy you're using
$ReferencePolicyFilePath = 'ReferencePolicy.xml'

# Name this whatever you want
$MergedPolicyFilePath = 'ReferencePolicyWithMitigations.xml'

# Parse the rules from the denial policy
$DenyRules = Get-CIPolicy -FilePath $DenialPolicyFilePath

# Merge the rules into a new, merged code integrity policy
Merge-CIPolicy -OutputFilePath $MergedPolicyFilePath -PolicyPaths $ReferencePolicyFilePath -Rules $DenyRules
```

For additional background on creating and merging deny rules, please refer to my [blog post](http://www.exploit-monday.com/2016/09/using-device-guard-to-mitigate-against.html) on the subject.
