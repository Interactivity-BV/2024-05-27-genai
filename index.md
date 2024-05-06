---
layout: workshop      # DON'T CHANGE THIS.
# More detailed instructions (including how to fill these variables for an
# online workshop) are available at
# https://carpentries.github.io/workshop-template/customization/index.html
venue: "Fort de Gagel"        # brief name of the institution that hosts the workshop without address (e.g., "Euphoric State University")
address: "Gageldijk 167, 3566 MJ Utrecht"      # full street address of workshop (e.g., "Room A, 123 Forth Street, Blimingen, Euphoria"), videoconferencing URL, or 'online'
country: "The Netherlands"      # lowercase two-letter ISO country code such as "fr" (see https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes) for the institution that hosts the workshop
language: "en"     # lowercase two-letter ISO language code such as "fr" (see https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) for the workshop
latitude: "52.129644351029945"        # decimal latitude of workshop venue (use https://www.latlong.net/)
longitude: "5.102454913794029"       # decimal longitude of the workshop venue (use https://www.latlong.net)
humandate: "May 27, 2024"    # human-readable dates for the workshop (e.g., "Feb 17-18, 2020")
humantime: "8:45 am - 5:00 pm"    # human-readable times for the workshop e.g., "9:00 am - 4:30 pm CEST (7:00 am - 2:30 pm UTC)"
startdate: 2024-05-27      # machine-readable start date for the workshop in YYYY-MM-DD format like 2015-01-01
enddate: 2024-05-27        # machine-readable end date for the workshop in YYYY-MM-DD format like 2015-01-02
instructor: ["Sven Warris"] # boxed, comma-separated list of instructors' names as strings, like ["Kay McNulty", "Betty Jennings", "Betty Snyder"]
helper: []     # boxed, comma-separated list of helpers' names, like ["Marlyn Wescoff", "Fran Bilas", "Ruth Lichterman"]
email: ["sven@interactivity.nl"]    # boxed, comma-separated list of contact email addresses for the host, lead instructor, or whoever else is handling questions, like ["marlyn.wescoff@example.org", "fran.bilas@example.org", "ruth.lichterman@example.org"]
collaborative_notes:  https://docs.google.com/document/d/1wzSEQpO0oQ-ZOVO72UQaYkc_UGN7WR1hEMaGu73fghI/edit?usp=sharing
eventbrite:           # optional: alphanumeric key for Eventbrite registration, e.g., "1234567890AB" (if Eventbrite is being used)
---


<h2 id="general">General Information</h2>


{% if site.carpentry == "swc" %}
{% include swc/intro.html %}
{% elsif site.carpentry == "dc" %}
{% include dc/intro.html %}
{% elsif site.carpentry == "lc" %}
{% include lc/intro.html %}
{% endif %}

{% comment %}
AUDIENCE

Explain who your audience is.  (In particular, tell readers if the
workshop is only open to people from a particular institution.
{% endcomment %}
{% if site.carpentry == "swc" %}
{% include swc/who.html %}
{% elsif site.carpentry == "dc" %}
{% include dc/who.html %}
{% elsif site.carpentry == "lc" %}
{% include lc/who.html %}
{% endif %}

{% comment %}
LOCATION

This block displays the address and links to maps showing directions
if the latitude and longitude of the workshop have been set.  You
can use https://www.latlong.net/ to find the lat/long of an
address.
{% endcomment %}
{% assign begin_address = page.address | slice: 0, 4 | downcase  %}
{% if page.address == "online" %}
{% assign online = "true_private" %}
{% elsif begin_address contains "http" %}
{% assign online = "true_public" %}
{% else %}
{% assign online = "false" %}
{% endif %}
{% if page.latitude and page.longitude and online == "false" %}
<p id="where">
  <strong>Where:</strong>
  {{page.address}}.
  Get directions with
  <a href="//www.openstreetmap.org/?mlat={{page.latitude}}&mlon={{page.longitude}}&zoom=16">OpenStreetMap</a>
  or
  <a href="//maps.google.com/maps?q={{page.latitude}},{{page.longitude}}">Google Maps</a>.
</p>
{% elsif online == "true_public" %}
<p id="where">
  <strong>Where:</strong>
  online at <a href="{{page.address}}">{{page.address}}</a>.
  If you need a password or other information to access the training,
  the instructor will pass it on to you before the workshop.
</p>
{% elsif online == "true_private" %}
<p id="where">
  <strong>Where:</strong> This training will take place online.
  The instructors will provide you with the information you will need to connect to this meeting.
</p>
{% endif %}

{% comment %}
DATE

This block displays the date and links to Google Calendar.
{% endcomment %}
{% if page.humandate %}
<p id="when">
  <strong>When:</strong>
  {{page.humandate}}.
  {% include workshop_calendar.html %}
</p>
{% endif %}

{% comment %}
SPECIAL REQUIREMENTS

Modify the block below if there are any special requirements.
{% endcomment %}
<p id="requirements">
  <strong>Requirements:</strong>
  {% if online == "false" %}
    Participants must bring a laptop with a
    Mac, Linux, or Windows operating system. You need to have a modern Internet browser. If you'd like, you can use Visual Studio Code + Jupyter Notebook plugin as. 
  {% else %}
    Participants must have access to a computer with a
    Mac, Linux, or Windows operating system (not a tablet, Chromebook, etc.) that they have administrative privileges on.
  {% endif %}
</p>

<p id="who-can-attend">
    <strong>Who can attend?:</strong>
    This workshop is open to members and associate members of the Virtual Lab for Plant Breeding (VLPB). There is a limit of 14 participants. 
</p>

<hr/>


{% if page.collaborative_notes %}
<h2 id="collaborative_notes">Collaborative Notes</h2>

<p>
We will use this <a href="{{ page.collaborative_notes }}">collaborative document</a> for chatting, taking notes, and sharing URLs and bits of code.
</p>
<hr/>
{% endif %}

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

<hr/>


<h2 id="setup">Setup</h2>

<div id="ChatGPT">
<h3>OpenAI ChatGPT</h3>
<p>
For this workshop we will be using <a href="https://chatgpt.com">ChatGPT</a> web interface for prompt engineering and testing ideas.
In Python we will be using the <a href="https://platform.openai.com/docs/api-reference?lang=python">OpenAI API</a>. A temporary API key will be supplied before the workshop. This key will expire after the workshop.</p>
<p>
A subscription to either ChatGPT or the OpenAI API is recommended, but not necessary. The ChatGPT 3.5 version will be sufficient and a key will be provided.</p>
<p>
Although we focus on ChatGPT, the concepts such as prompt engineer, API access and retrieval augmented generation also apply to many Open Source models found on <a href="https://huggingface.co/">Hugging Face</a>, such as llama 3, the new powerful LLM provide by <a href="https://huggingface.co/meta-llama">META</a>.</p>


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
