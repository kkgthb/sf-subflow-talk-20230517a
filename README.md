# Clone this and follow these steps to follow along

## Hand-create a `/.sfdx/sfdx-config.json` file

First things first:  you won't want to make it part of the "source code" for the CumulusCI project you're building, but every folder containing a CumulusCI project should have a `/.sfdx/sfdx-config.json` file in containing a single [JSON-formatted object](https://katiekodes.com/intro-xml-json-1/) that has exactly 1 property called `defaultusername` in it.  The value of `defaultusername` should be the alias that you gave a given Salesforce production org or developer org that you enabled "dev hub" functionality in when you logged the [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli), as installed on **your** computer, into that org.  You would have used the command `sfdx force:auth:web:login --setalias your-company-hub-org-nickname --instanceurl https://customdomain.my.salesforce.com/`.  You can see what you called it when you logged in by looking under the "Alias" column of the results of `sfdx auth:list`.

Here's an example `/.sfdx/sfdx-config.json` file:

```json
{
    "defaultdevhubusername": "your-company-hub-org-nickname"
}
```

If you see an entry under `sfdx auth:list` that you'd like to use but it has a blank value under "Alias," take the value under "username" and plug it into the `sfdx alias:set your-company-hub-org-nickname=the-username-you-chose@example.com`, substituting in appropriate values for "`your-company-hub-org-nickname`" and "`the-username-you-chose@example.com`," of course.

**Note:**  You need to do this by hand after downloading a copy of this codebase.  I did not include a sample in this codebase.

---

## HAVE FUN:  Validate that CumulusCI works

### Build a scratch org

To validate that the "minimum viable build" I've described here is _still_ all you need to make CumulusCI capable of spinning up a scratch org from a project, try the following:

```sh
cci flow run dev_org --org your-scratch-org-nickname-here
```

_(Substitute `beta`, `dev`, `feature`, or `release` for "`your-scratch-org-nickname-here`.")_

### Open the scratch org

Once it's finished, spinning up the scratch org, you can open it in a web browser with the following command:

```sh
cci org browser --org your-scratch-org-nickname-here
```

Or, if your computer's default web browser and scratch orgs don't get along, you can make your computer's command line give you a URL to hand-copy-and-paste into a different web browser:

```sh
cci org browser --org your-scratch-org-nickname-here --url-only
```

### Delete the scratch org

Once you're done, if you need to delete the scratch org before it naturally expires, you can run the following command:

```sh
cci org scratch_delete your-scratch-org-nickname-here
```

### Update your scratch org from changed dependencies

On the other hand, if all you've done is update the `dependencies` sub-property of `project` inside of a `cumulusci.yml` file, rather than deleting and recreating the scratch org, typically all you have to do is run the following _(which is really handy if one of the dependencies you've already let install is slow, like [NPSP](https://www.salesforce.org/products/nonprofit-success-pack/))_:

```sh
cci task run update_dependencies --org your-scratch-org-nickname-here
```

Head back over to the running scratch org in your web browser and reload an appropriate page in Setup to see that your changes took effect.

### Update your scratch org from your codebase

And if all you've done is hand-write some new code into `/force-app/` or wherever it is you're putting your codebase, then you don't need to delete and recreate the scratch org.  You can just run this command:

```sh
cci task run dx_push --org your-scratch-org-nickname-here
```

Head back over to the running scratch org in your web browser and reload an appropriate page in Setup to see that your changes took effect.

### Update your codebase from your scratch org

That said, usually you'll be doing things the other way around:  clicking through the browser in your scratch org, reconfiguring Salesforce, and then pulling down text-based copies of your new-and-improved configuration into your project folder with the following commands:

```sh
cci task run list_changes --org your-scratch-org-nickname-here
```

```sh
cci task run retrieve_changes --org your-scratch-org-nickname-here
```

[Check out the talk](https://katiekodes.com/subflow-witmd23/)

-[Katie Kodes](https://katiekodes.com/)