# WSL Configs

# About

This repo contains misc configuration for Woodside Lights, an automated light show in Ashland, VA.

## Visitor Tracking

This solution is used for tracking *approximate* visitor attendance by means of Unifi Protect object detection, FPP & Home Assistant.

### Prerequisites:

- Active HASS environment
- FPP instance online on local network
- Unifi Protect (with Gen4 or newer cameras) is integrated with Home Assistant
- Non-overlapping object detecton zones are defined on each camera to be monitored for visitor tracking. Non-overlapping object detection zones (I'm using 70% confidence on the zones, ymmv) are important to cut down on duplicate counts.

### The premise of operation is below:

- The HASS rest plugin is used to query the status of a master FPP instance to get state information for the show. As it applies to this, the current playing playlist is evaluated to infer whether the show is in a running state or not. A boolean helper entity is used and updated to reflect this for ease of further processing, both for visitor detection and showtime house light control. You can edit the automation to fit your needs including removing this intermediate helper if not needed.
- Assuming a Unifi Protect instance is integrated with HASS and Gen4 (or newer) cameras are used, object detection events are automatically collected and stored for each camera.
- A automation with crude debouncing is used in Home Assistant to consume these object detection events when triggered for >=5 seconds from Protect and increment 2 separate counter helpers (one for vehicles, one for people) in HASS __ONLY__ when the show is running (again, to cut down on false counts).

### Files:

- ./visitor-tracking/rest_plugin_fpp.yaml - REST plugin configuration for scraping FPP status from API
- ./visitor-tracking/auto_visitor_detected.json - Configuration for automation which runs the tracker.

### Not Shown:

- The helpers you need to configure (easy to infer/update from configs in UX)
- Dashboard configuration (once you have the counter entities defined, easy enough to add cards for them to a dashboard)

### Misc:

- If you are using the 'Show Status' boolean helper, you can use it in other automations as well. For example, I have an automation which shuts off select 'visitor facing' inside/outside lights if they are on, or turned on, during showtime. The family *LOVES* this.. 🤪
