## account_config
Account config files referenced by devops code for upstream and downstream builds, and baking. May also be used a single source of truth for users, groups, roles, and other pertinent cloud infra dependent config details for market data cloud.

### Index

Name | Folder | Description
---- | ------ | -----------
Bake Files | [bake_files](#bake_files) | referenced by image repos for default packages
Pipeline Files | [pipeline_files](#pipeline_files) | reference by the pipeline stack for centralized canonical config data
   
-------------

### `bake_files`

 * stub json files specified inside bake.json image repos to install a currated group of packages. refer to the [template repos](https://github.factset.com/search?q=topic%3Aqt-templates+org%3Amarket-data-cloud+fork%3Atrue) for more details.
 
 * stub bake files are ideal for packages that are not to be specified by developers but are required for a large group of images. this minimizes margin of error for developers as the stub files may now contain a static group of packages and their appropriate versions; the developer can simply *stub* it into their bake.json.

**Editing JSON Bake File**
 * Change the following pamaters for the bake.json file in the root directory of the repo as follows:
    * "name"
      * only specify packages that are certain not to be included by developers inside their image repo bake.json's otherwise baking will fail when this file is stubbed in.
      * replace with the name of the package repo, required as part of image bake.
      * i.e, 'FDSCbase_config.pkg'
    * "version"
      * version of the package desired for bake. semantic versioning with wild cards only are accepted.
      * i.e., 'v1.\*.\*' or 'v1.1.1'
    * "git_org"
      * May be omitted, in which case defaults to 'market-data-cloud'.
 * seperated by `{ },` with a comma excluded for the last item, add a new map for each package to be installed as part of bake and runtime. Each item inside the map excluding the last, should be seperated by a comman (,).
 
#### Example

filename: org_default.json
 
 ```
{
  "packages": [
    {
      "name": "FDSCbase_configs.pkg",
      "version": "v1.2.0"
    },
    {
      "name": "FDSCzml_config.pkg",
      "version": "v1.2.0"
    },
    {
      "name": "FDSCmonitor_client.pkg",
      "version": "v1.2.1"
    }
  ]
} 
 ```
 
 ### `pipeline_files`

 * json files containing a map of maps for data parsing by the pipeline stack. 
 * removes the need to input or amend various data parmeters multiple times across the pipeline stack.

**Editing JSON Pipeline File**
 * a map of maps each containing the nested map name and key/value pairs.
    * { "map name 1": {"key 1":"value 1", "key 2":"value 2"}, "map name 2": {"key 1":"value 1", "key 2":"value 2"} }
    * note: commas are always ommited for the last entry.
 * each nested map may be parsed based on the needs of the stack.
   * for example, a `region_abbr_map` map containing region names and their abbrevited values for use by Jenkins for a more compact pipeline name. Seperately, the `account_map` ma be used by Jenkins to convert account names to account ID numbers for assume role functions.  
 
#### Example

filename: aws_reference_map
 
 ```
{
 "account_map": {
   "content.qt-bdf.dev": "041769286182",
   "content.qt-build.dev": "613471279116",
   "content.qt-build.uat": "525934800244",
   "content.qt-build.ssvc": "000892896222"
  },
 "account_abbr_map": {
   "content.qt-bdf.dev": "bdf.dev",
   "content.qt-build.dev": "build.dev",
   "content.qt-build.uat": "build.uat",
   "content.qt-build.ssvc": "build.ssvc"

  },
 "region_abbr_map": {
   "us-east-1": "use1",
   "us-east-2": "use2"
  }
}
 ```
