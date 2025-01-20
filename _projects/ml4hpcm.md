---
layout: page
title: ML4H Process and Content Management System #a project title as it will appear on the website
img: /assets/img/ML4HPCM.png #thumbnail logo for the project overview
img2: /assets/img/AIMDSCOPE.png #a second banner for the second half of the page, contents of this banner should be related to the work stream
worktype: bg-primary #select one of the colors (researchandmethods: bg-success, standardizationandregulation: bg-primary, softwaretooling: bg-info)
lifecyclesteps: 1 2 3 4 #select one or more of the numbers for the life cycle steps 1 2 3 4
description: A unified modelling framework to address the problem of conceptual mapping and semantic interoperability of product requirements of AI/ML based medical devices among various stakeholders including software developers, quality managers, medical professionals and notified bodies. #a very short description of
projecttag: ml4hpcm #use this tag as the name for your project .bib file
contact: Christian Johner, christian.johner@johner-institut.de / Pradeep Balachandran, pradeep@aiaudit.org #Firstname Lastname, email of the general contact
coordinates: Bi-weekly Tuesday, 2.00 PM, CET #Interval and day, HH:MM PM, time zone
zoomroom: https://zoom.us/j/97186065542?pwd=MG9JeUpMYTYzNnhzb0FqMFloc2p2QT09 #link to the zoom room that is used for meetings
groupchat: https://daisamreggmlp-inx4514.slack.com #invite/access request link to the group chat
mailinglist: #email of the mailing list that people can subscribe to for this workstream
github: #provide a github link for the project if it exists
whiteboard: #link to the miro whiteboard that is used for the work stream
drive: #link to the shared drive of the work stream (for documents etc)
importance: 2
---
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ page.img | relative_url }}" alt="" title="" width="{{ site.max_width }}" height="100"/>
    </div>
</div>
<br/>

# Scope
This project aims at the design and development of a process and content management system(PCMS) for regulatory good machine learning practice guidelines(GMLP). The PCM system can potentially serve as a workflow utility or tool for the healthcare AI product developers, manufactiurers and regualtors and can guide them on how to efficiently and optimally adopt the regulatory GMLPs in reducing the complexity of the requirements analysis and conformity assessment workflows.

#### Aims
* To develop an ontology database

* To define processes for database content update

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
