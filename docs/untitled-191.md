# Assign a policy to organizations

## Assign a policy to organizations

A policy can be applied to multiple organizations; however, an organization can only be assigned to one policy. Organizations cannot be added to or removed from the default policy directly from the default policy modal.

Policies assigned to organizations will also apply when running snyk test or snyk monitor in the CLI.

### To add an organization to a policy:

Click on the associated check box from the organization selector panel.

If this organization is already assigned to another policy, you will be able to see which policy it is applied to from this selector and the pill next to the organization name will be grey

If an organization is assigned to the policy you are currently viewing, that policy name will appear in a yellow pill next to the organization name

If you are moving an organization from one policy to another, 2 pills will appear to show which policy is currently applied \(yellow\) and which policy you will be applying \(grey\)

### To remove an organization from a policy:

Uncheck the desired organization check box from the organization selector panel.

The unchecked organization\(s\) will now revert back to the default policy.

### Add an organization to the default policy:

1. Remove the organization from the policy it is currently assigned to
2. The organization will automatically move to the [default policy](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360007476397/README.md)

### Remove an organization from the default policy

1. Assign the relevant organization to a new policy 
2. The organization will automatically disassociate from the [default policy](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360007476397/README.md) and move to the newly selected policy
