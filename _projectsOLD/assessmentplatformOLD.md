---
layout: page
<<<<<<< HEAD:_projects/assessment-platform.markdown
title: Assessment Platform (Evaluation & Reporting Packages) #a project title as it will appear on the website
img: /assets/img/ap_logo.png #thumbnail logo for the project overview
img2: /assets/img/ap_logo.png #a second banner for the second half of the page, contents of this banner should be related to the work stream
worktype: bg-primary #select one of the colors (researchandmethods: bg-success, standardizationandregulation: bg-primary, softwaretooling: bg-info)
lifecyclesteps: 1 2 3 4 #select one or more of the numbers for the life cycle steps 1 2 3 4
description: Assessment Platform(AP) is an international initiative to create an open-source to promote health analysis.
projecttag: ap #use this tag as the name for your project .bib file
contact: Elora Schöerverth, elora@aiaudit.org), Pradeep Balachandran (pradeep@aiaudit.org) & Alixandro Werneck (alixandro@aiaudit.org)
coordinates: Weekly, Tuesday, 4:00 PM, CET #Interval and day, HH:MM PM, time zone
zoomroom: https://zoom.us/j/93853445188?pwd=cHpWTUZpQ3NWNnBzWDlZemlTUURyUT09 #link to the zoom room that is used for meetings
groupchat: https://discord.gg/3RZEQfM6 #invite/access request link to the group chat
mailinglist: #email of the mailing list that people can subscribe to for this workstream
github: #provide a github link for the project if it exists
whiteboard: #link to the miro whiteboard that is used for the work stream
drive: #link to the shared drive of the work stream (for documents etc)
importance: 2
=======
title: Assessment Platform
description: Assessment Platform(AP) is an international initiative to create an open-source to promote health analysis.
img: /assets/img/ap_logo.png
contact: Elora Schöerverth (elora@aiaudit.org), Pradeep Balachandran (pradeep@aiaudit.org) & Alixandro Werneck (alixandro@aiaudit.org)
coordinates: Every Tuesday, 4.00 PM, CET
importance: 3
>>>>>>> 16da9487859e939baa8b20cd98b8da8f53d04f01:_projects/assessmentplatform.md
worktype: bg-info
---
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ page.img | relative_url }}" alt="" title="" width="{{ site.max_width }}" height="100"/>
    </div>
</div>
<br/>

# Scope

Assessment Platform(AP) is an international initiative to create an open-source to promote health analysis. This Open Code Project aims to produce the digital building blocks (six software packages) that compose the FG-AI4H Assessment Platform. The assessment platform, which distinguishes it from AI "challenge" platforms through its consideration of regulatory guidelines and the needs of other AI for health stakeholders, supports the end-to-end assessment of AI for health algorithms under Marc Lecoultre's coordination.

AI Audit is contributing with two software packages of the AP: Evaluation and Reporting.

_______________________________________________________

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ep_logo.png' | relative_url }}" alt="" title=""  width="{{ site.max_width }}" height="100"/>
    </div>
</div>

# Evaluation Package

# Project Scope

Model performance is dependent on the choice of metric and possible parameters. Thus, it is of utmost importance to have a framework that allows for comparing the performance of different AI models. EP provides meaningful, state-of-the-art metrics that promise high expressibility.

#### Aims
* Offers testing measures and methods for different quality dimensions, including interpretation, bias, uncertainty, and robustness.
* Questionnaires provide a qualitative evaluation

#### Outputs
<div class="publications">
  {% bibliography -f {{ page.projecttag }} -q @*[projectoutput=true]* %}
</div>

---
# Collaboration resources
You are welcome to inquire about the work stream and opporunities for collaboration directly with the work stream team.
* **General contact** {{ page.contact }}

#### Meetings
Regular meetings for this work stream take place at the below coordinates.
* **Meetings** {{ page.coordinates }}
* **Zoom room** [Click here to join meeting]({{ page.zoomroom }})

#### Communication
You can subsbscribe to the work stream mailing list to receive updates and join the asynchronous group chat.
* **Group chat** {{ page.groupchat }}
* **Mailing list** {{ page.mailinglist }}

#### Tools
We use different tools in our remote work. They include shared documents, github projects for code as well as task tracking and a collaborative whiteboard for ideation. You can request access via the below links.
* **Shared drive** {{ page.drive }}
* **Github project** {{ page.github }}
* **Collaborative whiteboard** {{ page.whiteboard }}
<iframe width="768" height="432" src="https://miro.com/app/live-embed/o9J_lSKa6_w=/?moveToViewport=-1880,-657,3491,1785" frameBorder="0" scrolling="no" allowFullScreen></iframe>

You can find more information about the way we usually carry out our work remotely in teams [here](https://aiaudit.org/join).

_______________________________________________________

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rp_logo.png' | relative_url }}" alt="" title="" width="{{ site.max_width }}" height="100">
    </div>
</div>

_______________________________________________________
# Reporting Package

# Scope
RP delivers a standardized format for communicating and reporting the properties, features, intended use, and limitations of AI for health to help connect different stakeholders.

#### Aims
A customizable reporting interface that presents the results of EP.

#### Outputs
A software package to manufacturers, notified regulatory bodies, AI users for health (doctors, patients), and vendors of AI for health.

---
# Collaboration resources
You are welcome to inquire about the work stream and opporunities for collaboration directly with the work stream team.
* **General contact** {{ page.contact }}

#### Meetings
Regular meetings for this work stream take place at the below coordinates.
* **Meetings** {{ page.coordinates }}
* **Zoom room** [Click here to join meeting]({{ page.zoomroom }})

#### Communication
You can subsbscribe to the work stream mailing list to receive updates and join the asynchronous group chat.
* **Group chat** {{ page.groupchat }}
* **Mailing list** {{ page.mailinglist }}

#### Tools
We use different tools in our remote work. They include shared documents, github projects for code as well as task tracking and a collaborative whiteboard for ideation. You can request access via the below links.
* **Shared drive** {{ page.drive }}
* **Github project** {{ page.github }}
* **Collaborative whiteboard** {{ page.whiteboard }}

You can find more information about the way we usually carry out our work remotely in teams [here](https://aiaudit.org/join).

---

# Milestones
<div class="news">
  {% if site.news  %}
    <div class="table-responsive">
      <table class="table table-sm table-borderless">
      {% assign news = site.news | reverse %}
      {% for item in news limit: site.news_limit %}
        {% if item.projecttag == page.projecttag %}
            <tr>
            <th scope="row">{{ item.date | date: "%b %-d, %Y" }}</th>
            <td>
                {% if item.inline %}
                {{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
                {% else %}
                <a class="news-title" href="{{ item.url | relative_url }}">{{ item.title }}</a>
                {% endif %}
            </td>
            </tr>
        {% endif %}
      {% endfor %}
      </table>
    </div>
  {% else %}

  {% endif %}
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ page.img2 | relative_url }}" alt="" title="" width="{{ site.max_width }}" height="100"/>
    </div>
</div>
<br/>

# Important reference material
This is a list of related work and resources relevant for this work stream. It comprises resources the work stream contributors consider good practice.

<div class="publications">
  {% bibliography -f {{ page.projecttag }} %}
</div>
