# Automating Deployment in Azure DevOps

Contents

Introduction        1

Some assumptions        1

Keeping track of version numbers        2

How to release        3

Getting fancy with Power Shell        3

Automate using DevOps Boards        3

Conclusion        5



# Introduction

Cloud. DevOps.  Automated build and release pipelines.  Buzzwords, buzzwords, buzzwords.  How do I make these buzzwords become something real?  Making it reality instead of some pie in the sky.  What are the best practices? How do I do this stuff?  Questions, questions, questions, no clear answer.  Lots of documentation and nothing with practical direction.  Advice saying there is no right and wrong answer and any way that works for you…really does not help.  Confused, frustrated, nowhere.

What does help Is when someone say, &quot;This is how I do it&quot; and then show you how they did it.  It can make a world of difference.

I have been lucky.  I have worked in various industries from small businesses to large multinational enterprises doing software development ranging from support &amp; maintenance to brand spanking new solutions. I have learned some stuff from some smart people over the years.  This has given me some unique insights and perspectives.

We need a disclaimer before I continue.  I am about to share my thoughts and opinions. It might differ from yours.  Your circumstances might be very different.  You might simply not agree with me.  It is all OK but, before you go all negative, give it some real thought.  I will try to explain things a clearly as possible and hopefully add some value to your current knowledge base.

Now let me share how I do it.

# Some assumptions

For the purpose of this article I will assume the following:

- Source control is Git in Azure DevOps
- Code is C# using either .Net Framework 4.8 or .Net Core 3.1
- PowerShell will be used for CLI (Command Line Interface) commands.
- Git Branch Policies are used that enforces Pull Requests with an associated build
  - The build includes Unit Tests
- Releases are done from master
  - Not going to cover branch management and what way is better for what reason
  - I prefer using
    - short living feature branches
    - releasing from Tags and not release branches
      - my preferences for this should become clear as explain the rest.

The above assumptions provide the foundations for the automated deployment of your code in Azure DevOps.

# Keeping track of version numbers

First thing you need is a way to keep track of your version numbers.  For this I use GitVersion (http://....).  This tool keeps track of version numbers through Git Tags based on Semantic Versioning (http://...).  SemanticVersioning is based on Major.Minor.Patch where

Major = Major breaking changes

Minor = most changes

Patch =

GitVersion can also update all the assemblies to have the same version compared to the version being deployed.

How to use?

1. Add a GitVersion.yml configuration file to the root of your repository with the following content

assembly-versioning-scheme: MajorMinorPatch

assembly-informational-format: &#39;{MajorMinorPatch}&#39;

branches:

  master:

    regex: master

    increment: Minor

  hotfix:

    regex: hotfix(es)?[/-]

    increment: Patch

mode: ContinuousDelivery

next-version: &quot;0.0.0&quot;

Some notes on this configuration:

-
  -

1. Add the GitVersion task to the build pipeline in Azure DevOps and point it to the above GitVersion.yml file.
  1. GUI Screen shot
  2. Yml code
2. Setup the build triggers
  1. GUI screenshot
  2. YML screenshot
  3. Yml Gui screenshot
3. Setup the release triggers
  1. Gui screenshot



# How to release

So how do I now do the release?  Easy, you just create a Tag on the appropriate branch naming the tag according to the semantic versioning rules.

- Normal release: create tag on master.
- Hot fix release: create tag on hotfix branch.
- Long running feature: create tag on that feature branch.

Create a Tag in Azure DevOps:

- Screenshots

Ok, so now we have all this stuff up and running and you can create a release that automatically build and deploy your code using Azure DevOps.  You have also automated the version number management apart from manually creating the Tag.

Keep in mind that you will always have to take a manual action to trigger a release.  Now, let&#39;s get fancy.  Now that we have a basis on how we can trigger the release, what else can we do?



# Getting fancy with Power Shell

One option would be to create a PowerShell script to do this where you can execute the script from your local pc and it then go and do this.  This is however not recommended for the following reasons:

- You need to give the developer &quot;Override branch policies&quot; permissions.
  - Developer can now push anything to master
  - Not a big issue for a small experienced team, but not recommended for a large or inexperienced team.



# Automate using DevOps Boards

Consider the following approach.  Use tasks on the boards to trigger the appropriate release when a pull request is approved and completed.  These tasks can be general tasks or specific to a PBI.  What is need to do this?

- We would need a process to evaluate the associated task name to determine if there are any appropriate release tasks associated with the pull request.
- If there is such a task, trigger a process to go and create the Tag based on the determined version number from GitVersion.

Luckily for us, Azure DevOps have REST API methods available for us to call.

Checking for release task:

In the build pipeline, add the following build tasks:

git ….get

POWERSHELL TASK



Trigger the release:

In the build pipeline, add the following build tasks:

git ….get

# Conclusion

You are welcome to ignore all I have said, or adapt some of this to suite the way you want things to work, or you can do exactly this and experience some of the joy of having fully automated build and deployment pipelines and end up with more freedom to code and less worries about taking stuff live.

More code. Less admin. More release confidence.  Less worries.