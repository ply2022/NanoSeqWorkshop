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
humandate: "Aug 6th, 2022"    # human-readable dates for the workshop (e.g., "Feb 17-18, 2020")
humantime: "8:00 am - 5:00 pm"    # human-readable times for the workshop e.g., "9:00 am - 4:30 pm CEST (7:00 am - 2:30 pm UTC)"
timezone: "Eastern time"    # human-readable times for the workshop
startdate: 2022-08-06      # machine-readable start date for the workshop in YYYY-MM-DD format like 2015-01-01
enddate: 2022-08-06        # machine-readable end date for the workshop in YYYY-MM-DD format like 2015-01-02
organizer: ["Pei-Ling Yu", "Jeremy T. Brawner"]
instructor: ["Abigail Stack", "Kacie Quello", "Andrew J. Gitto", "Owen H. Hudson", "Joshua L. Konkol"] # boxed, comma-separated list of instructors' names as strings, like ["Kay McNulty", "Betty Jennings", "Betty Snyder"]
email: ["plyu@ufl.edu", "jeremybrawner@ufl.edu"]    # boxed, comma-separated list of contact email addresses for the host, lead instructor, or whoever else is handling questions, like ["marlyn.wescoff@example.org", "fran.bilas@example.org", "ruth.lichterman@example.org"]
technical: ["plyu@ufl.edu"]    # email addresss for technical help
collaborative_notes:  # optional: URL for the workshop collaborative notes, e.g. an Etherpad or Google Docs document (e.g., https://pad.carpentries.org/2015-01-01-euphoria)
eventbrite: false           # optional: alphanumeric key for Eventbrite registration, e.g., "1234567890AB" (if Eventbrite is being used)
---

<h2 id="general">General Information</h2>

{% comment %} Introduction {% endcomment %}
{% include intro/intro.md %}

{% comment %} Audience {% endcomment %}
{% include intro/who.md %}

{% comment %} Address {% endcomment %}
{% include intro/where.md %}

{% comment %} Date and Time {% endcomment %}
{: #when}
**When:** {{page.humandate}}, {{page.humantime}} {{page.timezone}}.

{% comment %} Prerequisite {% endcomment %}
{% include intro/prereq.md %}

{% comment %} Accessibility {% endcomment %}
{: #accessibility}
**Accessibility:**
We are dedicated to providing a positive and accessible learning environment for all. Please
notify the instructors in advance of the workshop if you require any accommodations or if there is
anything we can do to make this workshop more accessible to you.

{% comment %} Contact {% endcomment %}

[//]: # (Contact)
{: #contact}
**Contact:**
Please email 
{% for email in page.email %}{% if forloop.last and page.email.size > 1 %} or {% else %}{% unless forloop.first %}, {% endunless %}{% endif %} [{{email}}](mailto:{{email}}) {% endfor %}
for more information about the content.
For technical problems regarding the website,
email [{{ page.technical }}](mailto:{{ page.technical }}).

<hr/>


{% comment %} Survey {% endcomment %}
<h2 id="surveys">Surveys</h2>
<p>Please be sure to complete these surveys after the workshop.</p>
<p><a href="{{ site.baseSite }}{{ site.feedback }}">Post-workshop Survey</a></p>
<hr/>

{% comment %} Schedule {% endcomment %}
<h2 id="schedule">Schedule</h2>
{% include intro/schedule.md %}
<hr/>

{% comment %} Setup {% endcomment %}
<h2 id="setup">Setup</h2>
<p>Go to <a href="setup.html">this page</a> for setup instructions.
