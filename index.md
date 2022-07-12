---
layout: workshop      # DON'T CHANGE THIS.
# More detailed instructions (including how to fill these variables for an
# online workshop) are available at
# https://carpentries.github.io/workshop-template/customization/index.html
venue: "Plant Health 2022"        # brief name of the institution that hosts the workshop without address (e.g., "Euphoric State University")
address: "David L. Lawrence Convention Center, Pittsburgh, Pennsylvania." # full street address of workshop (e.g., "Room A, 123 Forth Street, Blimingen, Euphoria"), videoconferencing URL, or 'online'
country: " us"      # lowercase two-letter ISO country code such as "fr" (see https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes) for the institution that hosts the workshop
language: "en"     # lowercase two-letter ISO language code such as "fr" (see https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) for the workshop
latitude: "40.44576763101142, "        # decimal latitude of workshop venue (use https://www.latlong.net/)
longitude: "-79.99581233305766"       # decimal longitude of the workshop venue (use https://www.latlong.net)
humandate: "Aug 6, 2022"    # human-readable dates for the workshop (e.g., "Feb 17-18, 2020")
humantime: "8:00 am - 5:00 pm"    # human-readable times for the workshop e.g., "9:00 am - 4:30 pm CEST (7:00 am - 2:30 pm UTC)"
timezone: "Eastern time"    # human-readable times for the workshop
startdate: 2022-08-06      # machine-readable start date for the workshop in YYYY-MM-DD format like 2015-01-01
enddate: 2022-08-06        # machine-readable end date for the workshop in YYYY-MM-DD format like 2015-01-02
organizer: ["Pei-Ling Yu", "Jeremy T. Brawner"]
instructor: ["Abigail Stack", "Kacie Quello", "Andrew J. Gitto", "Owen H. Hudson", "Joshua L. Konkol"] # boxed, comma-separated list of instructors' names as strings, like ["Kay McNulty", "Betty Jennings", "Betty Snyder"]
email: ["plyu@ufl.edu", "jeremybrawner@ufl.edu"]    # boxed, comma-separated list of contact email addresses for the host, lead instructor, or whoever else is handling questions, like ["marlyn.wescoff@example.org", "fran.bilas@example.org", "ruth.lichterman@example.org"]
collaborative_notes:  # optional: URL for the workshop collaborative notes, e.g. an Etherpad or Google Docs document (e.g., https://pad.carpentries.org/2015-01-01-euphoria)
eventbrite:           # optional: alphanumeric key for Eventbrite registration, e.g., "1234567890AB" (if Eventbrite is being used)
---

<h2 id="general">General Information</h2>

{% comment %} Introduction {% endcomment %}
{% include intro/intro.md %}

{% comment %} Audience {% endcomment %}
{% include intro/who.md %}


{% comment %}
SURVEYS - DO NOT EDIT SURVEY LINKS
{% endcomment %}
<h2 id="surveys">Surveys</h2>
<p>Please be sure to complete these surveys before and after the workshop.</p>
{% if site.carpentry == "incubator" %}
<p><a href="{{ site.incubator_pre_survey }}">Pre-workshop Survey</a></p>
<p><a href="{{ site.incubator_post_survey }}">Post-workshop Survey</a></p>
{% elsif site.incubator_pre_survey or site.incubator_post_survey %}
<div class="alert alert-danger">
WARNING: you have defined custom pre- and/or post-survey links for
a workshop not configured for The Carpentries Incubator
(the value of `curriculum` is not set to `incubator` in `_config.yml`).
Please comment out the `incubator_pre_survey` and `incubator_post_survey` fields
in `_config.yml` or, if this workshop is teaching a lesson in the Incubator,
change the value of `carpentry` to `incubator`.
</div>
{% else %}
<p><a href="{{ site.pre_survey }}{{ site.github.project_title }}">Pre-workshop Survey</a></p>
<p><a href="{{ site.post_survey }}{{ site.github.project_title }}">Post-workshop Survey</a></p>
{% endif %}

<hr/>


{% comment %}
SCHEDULE

Show the workshop's schedule.

Small changes to the schedule can be made by modifying the
`schedule.html` found in the `_includes` folder for your
workshop type (`swc`, `lc`, or `dc`). Edit the items and
times in the table to match your plans. You may also want to
change 'Day 1' and 'Day 2' to be actual dates or days of the
week.

For larger changes, a blank template for a 4-day workshop
(useful for online teaching for instance) can be found in
`_includes/custom-schedule.html`. Add the times, and what
you will be teaching to this file. You may also want to add
rows to the table if you wish to break down the schedule
further. To use this custom schedule here, replace the block
of code below the Schedule `<h2>` header below with
`{% include custom-schedule.html %}`.
{% endcomment %}

<h2 id="schedule">Schedule</h2>

{% if site.carpentry == "swc" %}
{% include swc/schedule.html %}
{% elsif site.carpentry == "dc" %}
{% include dc/schedule.html %}
{% elsif site.carpentry == "lc" %}
{% include lc/schedule.html %}
{% elsif site.carpentry == "incubator" %}
This workshop is teaching a lesson in [The Carpentries Incubator](https://carpentries-incubator.org/).
Please check [the lesson homepage]({{ site.incubator_lesson_site }}) for a list of lesson sections and estimated timings.
{% endif %}

{% comment %}
Edit/replace the text above if you want to include a schedule table.
See the contents of the _includes/custom-schedule.html file for an example of
how one of these schedule tables is constructed.
{% endcomment %}

{% if site.pilot %}
The lesson taught in this workshop is being piloted and a precise schedule is yet to be established. The workshop will include regular breaks. Please [contact the workshop organisers](#contact) if you would like more information about the planned schedule.
{% endif %}

<hr/>


{% comment %}
SETUP

Delete irrelevant sections from the setup instructions.  Each
section is inside a 'div' without any classes to make the beginning
and end easier to find.

This is the other place where people frequently make mistakes, so
please preview your site before committing, and make sure to run
'tools/check' as well.
{% endcomment %}

<h2 id="setup">Setup</h2>

<p>
  To participate in a
  {% if site.carpentry == "swc" %}
  Software Carpentry
  {% elsif site.carpentry == "dc" %}
  Data Carpentry
  {% elsif site.carpentry == "lc" %}
  Library Carpentry
  {% endif %}
  workshop,
  you will need access to software as described below.
  In addition, you will need an up-to-date web browser.
</p>
<p>
  We maintain a list of common issues that occur during installation as a reference for instructors
  that may be useful on the
  <a href = "{{site.swc_github}}/workshop-template/wiki/Configuration-Problems-and-Solutions">Configuration Problems and Solutions wiki page</a>.
</p>

{% comment %}
For online workshops, the section below provides:
- installation instructions for the Zoom client
- recommendations for setting up Learners' workspace so they can follow along
  the instructions and the videoconferencing

If you do not use Zoom for your online workshop, edit the file
`_includes/install_instructions/videoconferencing.html`
to include the relevant installation instrucctions.
{% endcomment %}
{% if online != "false" %}
{% include install_instructions/videoconferencing.html %}
{% endif %}

{% comment %}
These are the installation instructions for the tools used
during the workshop.
{% endcomment %}

{% if site.carpentry == "swc" %}
{% include swc/setup.html %}
{% elsif site.carpentry == "dc" %}
{% include dc/setup.html %}
{% elsif site.carpentry == "lc" %}
{% include lc/setup.html %}
{% elsif site.carpentry == "incubator" %}
Please check the "Setup" page of
[the lesson site]({{ site.incubator_lesson_site }}) for instructions to follow
to obtain the software and data you will need to follow the lesson.
{% endif %}
