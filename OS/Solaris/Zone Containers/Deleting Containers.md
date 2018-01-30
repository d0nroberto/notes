# Deleting Solaris Zones

Deleting a zone is a relatively straightforward task.

We will simply remove the zone config and then clean up any associated storage.

First, check what zones are running on the global:

~~~~
# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   8 zoneB            running    /export/zones/zoneB            native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
~~~~

We want to delete zoneB, which is still running. Let's halt it with:

~~~~
# zoneadm -z zoneB halt

# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
   - zoneB            installed  /export/zones/zoneB            native   shared
~~~~

And now we will delete it with:

~~~~
# zoneadm -z zoneB uninstall -F
~~~~

Our final step is to clean up the zonepath storage that was allocated to the host.

~~~~
# zfs list | grep zoneB
export/zones/zoneB             1.65G  1.80T  1.65G  /export/zones/zoneB

# zfs destroy -R export/zones/zoneB
~~~~
