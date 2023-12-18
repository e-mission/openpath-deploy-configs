# Configuration files for current OpenPATH deployments

Publishing them on GitHub allows transparency around how they are configured.
This may not be the long-term solution, but it allows us to make progress over the short/medium term.

### Creating a new Config

Automation has been introduced for creating new config files. New deployers can now file a [templated issue](https://github.com/e-mission/nrel-openpath-deploy-configs/issues/new?assignees=&labels=new+config&projects=&template=add-new-config.yml&title=New+Project+Configuration+-+%5BPROJECT%5D) specifying their configuration, which will launch a GitHub Actions workflow with a pull request to add the new config to this repo. Additional steps may be required if they require additional customization of surveys or labels. 

Info for parthers: [in the docs](https://github.com/e-mission/e-mission-docs/tree/master/docs/use/start_a_project.md)
Info for developers: [in the docs](https://github.com/e-mission/e-mission-docs/tree/master/docs/dev/future/more_custom_auto_config.md)

### Reviewing and testing
- contact us by email at openpath@nrel.gov for access to staging apps (Android or IOS)
- also reach out to recieve an OPcode for stage study or stage program
  
#### Programs vs studies

NREL OpenPATH supports both _studies_ and _programs_. While most of the functionality is the same, studies and programs are subtly different - for example:
- studies support autogenerated tokens, while programs require a token from the program admin
- programs include a question for the mode that would have been used if the program intervention was not available

#### Stage study environment

If you are here to preview/review/beta test the app functionality, please use the stage study environment (see instructions above for requesting access)

#### Stage program environment

If you want to experiment with the stage environment for _programs_ please use the stage program environment (see instructions above for requesting access)

If you are here to test out the app on your personal phone, please use the open-access environment (https://open-access-openpath.nrel.gov/join)

In general, if you are planning to keep the app installed for less than a day, please use stage so you don't pollute the real dataset.

#### Testing configs

As we test more config options, we sometimes need to be able to edit and load configs locally without pushing to github
and waiting for a PR to be approved.

To accomplish this:
- Change the download URL in `www/js/config/dynamic_config.js` to `"http://localhost:9090/configs/"+label+".nrel-op.json"`
- Modify one of the existing configs **OR** create a new config and add it to `docs/index.html`
- `docker-compose -f docker-compose.dev.yml up -d`
- In the emulator, go to http://localhost:9090 and click on the appropriate link

### File format

Config format (with default values) is:

```
{
    "version": 1,
    "server": {
        "url": "https://openpath-stage.nrel.gov/api/",
        "aggregate_call_auth": "user_only"
    },
    "intro": {
        "program_or_study": "study",
        "program_admin_contact": "K. Shankari",
        "deployment_partner_name": "National Renewable Energy Laboratory (NREL)",
        "translated_text": {
            "en": {
                "deployment_name": "Open Access Study",
                "summary_line_1": "enables people to track their travel modes and measure their associated energy use and emissions",
                "summary_line_2": "makes aggregated data on mode shares, trip frequencies, and carbon footprints available via a public dashboard",
                "summary_line_3": "serves as a control group while evaluating behavior change of programs.",
                "short_textual_description": "Transportation is the largest contributor to ...",
                "why_we_collect": "NREL can use this information to ....",
            },
            "es": {
....
            },
            "support_autogen_token": true,
        }
    }
    "display_config": {
        "use_imperial": false,
    },
    "profile_controls": {
        "support_upload": false,
        "trip_end_notification": false
    }
}
```

### Development

This repo has some simple GitHub pages in the `docs` repo

If you want to experiment with them (e.g. by changing the format or the URL
prefix), you can use the attached `docker-compose.yml` to serve the pages
locally at http://localhost:9090

I found this useful while testing the QR code functionality on the devapp,
which responds to the `emission` URL prefix, not `nrelopenpath`