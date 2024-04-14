
## Site Versioning Notes

As the PR is long and a bit hard to read, I thought to summarise some of the conclusions here in markdown that would be easy to copy and paste out later.

<box type=info>
   
  Add a command to archive a version under a given name. Archived versions are stored in HTML format, in a folder with the version name, which is inside the version folder(default name for the version folder is "version"). All links in the versioned site will link to other pages in the versioned site.

  To summarize the current solution, what it does is build the website and place it into a folder within the repository for it to be versioned (default location is version/, but you can customise this). When the site is built, by default the build action copies the versioned site over into _site, and it is deployed. If the user decides to store versions in different folders, then the URL to the version will simply be different.

</box>

Benefits of the solution proposed in this PR

  * Simple & customisable
  * Since .html and asset files are kept, past changes in markbind version, assets used, etc will be no issue --> each version contains everything it needs to deploy as a site
  * (Relatively) performant. Copying the files of html folders/asset folders should be a relatively small cost, as compared to generating from markbind files.
    * warn for potential size inflation when site is versioned

> Simple usecase:
> A documentation website for your project with x3 versions. 1 version is no longer supported by you so you don't want it linked on your website but you don't want to delete it. You want to keep 2 versions on your website for users to reference - in addition to the current version.

Components of the solution proposed
* New versions.json stores information about the version (name, location, markbind version, baseurl(?))
* Each version's internal links should point internally --> so baseurl should be modified accordingly

<panel header="Should users be able to edit the archived site?">

Referencing our usecase, it is clear you may sometimes want to edit your versioned site. For example, if you cherrypick a bugfix into your past version of your project, you may want to update that file.

However, we only store HTML files in the new website.
</panel>

Requirements met:
- [ ] readers should be able to navigate to newer version from older versions - hence a single source of truth for versions is needed for all versions. (e.g. a versions.json output metadata file at the output (_site) and source folder root)
- [ ] the data file structure / design used to store versioning information should be extensible, so we can add more versioning information in the future as needed without breaking past versions
- [ ] a markbind archive "version name" command
- [ ] re-executing this command should replace the version with a newly built one (see note on baseUrl below), _so users can change the baseUrl_
<!-- was this actually fixed? -->
- [ ] a markbind "delete-archive" "version name" command (can be done separately)
- How will this work with sub-sites? (can consider for later)

Requirements not met (delayed for later)
- [ ] a simple frontend versioning component should be provided, that allows readers to navigate to any version from any version (can be done separately)
- Would we need a feature to revert to a previous version (stretch goal)
- 

### Alternative approaches and why they were not adopted

<panel header="Saving past versions as `.md` files">

> "If we were to allow editing of past versions, then I think saving the past versions as .md files will make more sense as it will be easier for the users to edit and be able to use MarkBind's syntax."


</panel>

<panel header="Why not just build subsites every time its built/deployed?">

* Performance hit
* MarkBind version issues - if a subsite was built with v3 MarkBind, it might not work with v5 MarkBind

</panel>

<panel header="How to improve performance?">

<panel header= "hannah - gitbased versioning" type="info">
Gitbranch based site versioning --> Would be reliant on Git

"A potential workaround would be saving each version in a separate branch, where the name is the version name. Then the deploy command would keep track of the commit hashes where each version was saved, and use that branches files to generate html files to place in gh-pages. To lessen the time for the build command, deploy could be made more intelligent so that versioned files are only replaced if the commit hash has been changed since the last time. An additional feature that would need to be implemented is some command to 'build past version files' in order to produce the past version files in the current local branch."
</panel>
<panel header= "zeyu - design for versatile versioning" type="info" popup-url="https://github.com/MarkBind/markbind/issues/1009#issuecomment-1074299971">

</panel>

---
 sub-sites are only means for content reuse. (only the outer site's pages are deployed - which can, but not necessarily include the inner site's pages).