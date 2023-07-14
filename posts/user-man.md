---
title: Some thoughts on user managment 
date: 2023-07-14
---

# Some thoughts 
Coming from a background focused on software development, modern user management and system administration kinda shocked and left me mind blown since it's something I never gave a lot of thought to. Thus far my experience with user management before my current role was maybe having an admin view, and just general application users.

I never realized how intricate user management can become in a larger ecosystem. There are numerous fields and data points involved in managing users across networks and devices, which can make things quite messy. You have to handle and analyze a significant amount of activity, different user IDs across various applications or devices, and different permission levels. It can all get a bit overwhelming. Today, I want to break down some of the things I have recently read or experienced regarding user management for my own understanding and sanity (lol).

# User groups   

From what I understand, user groups serve as mechanisms or containers to manage permissions. Users can belong to multiple groups and gain the benefits of the group with the least restrictive permissions. However, managing users across multiple groups can become challenging and messy if there is no implemented system that allows you to see the overlap of group memberships. 

 # User permissions

Permissions enable users to access resources such as devices or applications within an ecosystem. Admins can manage users on a larger scale by assigning appropriate permissions based on their roles and needs. For example, some users may require read-only access and should not be able to modify data they do not work with. While this example may seem straightforward, permissions can become complex when you introduce various user needs into the equation.

# User Policies
Account roles/groups and their associated permissions help establish access control policies. Policies essentially provide directives on how users should be allowed to access resources.

 For instance, a company might create a policy stating that users in group A can only use a particular system/application if they belong to a specific department within the company network and are accessing it during work hours. In this case, a system admin would need to assign a role to users indicating their department, and assign specific roles to each department that grant or restrict access to specific systems. Additionally, they would need to include another field, such as user work hours, to ensure access is limited to work hours only. 
 
 Again, while this is a simple example, larger corporations with numerous scenarios, systems, and user groups face significant challenges in efficiently managing such complexities.

# Peace!
Overall user and application management gets even more complicated when you have different policies affecting users, applications and your global policies. There’s no way to easily visualize how the system you’re working in treats all these variables, but I find that drawing out how all these variables interact really helps better understand! A combination of venn diagrams, flow maps, and tree structures goes a long way.  Anyway, while I can probably keep spewing random stuff on user management I just wanted to let out some thoughts on it. Peace!


