# Beaker Test Harness for Fedora 31 / Rawhide

## Rationale
At this moment [Beaker harness repo](beaker-project.org/yum/harness) is not providing a harness for Fedora 31/Rawhide. This can be an issue when [Beaker](https://github.com/beaker-project/beaker) is used for provisioning. Beaker will abort all jobs without test harness. This behaviour can be overridden by ks_meta, but overall it doesn't make sense to run provisioning without test harness.

## How to use RPMs from this repo
There are several ways how you can leverage this repo  
1. Store RPMs in default beaker harness directory - not preferred
  * Fetch RPMs and store it in your harness directory in Beaker server [default: /var/www/beaker/harness].  
  * Create repository
    ```
    createrepo_c <directory_to_index> --no-database
    ```
  2. Host RPM(s) on HTTP server - preferred
  * Fetch RPM(s) and store them to exposed path  
  * Create repository
    ```
    createrepo_c <directory_to_index> --no-database
    ```  
  * Refer your exposed repo in Beaker Job XML:  
    ```xml
    <repos>
      <repo name="myharness" url="http://example.com/~martin/restraint/FedoraRawhide/"/>
    </repos>
    ```
    Update ks_meta variable
    ```
    ks_meta="harness='restraint-rhts' no_default_harness_repo"
    ```
    **Explanation**: [Beaker](https://github.com/beaker-project/beaker) is looking into all repositories (compose + custom one) to find test harness defined by variable *harness* in ks_meta.
    In this case, we still want to use *restraint(-rhts)*. ks_meta variable *no_default_harness_repo* is **required**, otherwise, scheduler will abort provisioning (due to missing test harness).  
  * Profit!
