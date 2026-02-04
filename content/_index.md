---
# Leave the homepage title empty to use the site title
title: 'Fuad Hasan'
date: 2022-10-24
type: landing

sections:
  - block: about.biography
    id: about
    content:
      title: About Me
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
#  - block: skills
#    content:
#      title: Skills
#      text: ''
#      # Choose a user to display skills from (a folder name within `content/authors/`)
#      username: admin
#    design:
#      columns: '1'
  - block: experience
    content:
      title: Experience
      # Date format for experience
      #   Refer to https://docs.hugoblox.com/customization/#date-format
      date_format: August 2022
      # Experiences.
      #   Add/remove as many `experience` items below as you like.
      #   Required fields are `title`, `company`, and `date_start`.
      #   Leave `date_end` empty if it's your current employer.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - title: Research Assistant
          company: Rensselaer Polytechnic Institute
          company_url: 'https://www.rpi.edu/'
          company_logo: rpi
          location: Troy, NY, USA
          date_start: '2023-12-21'
          date_end: ''

        - title: Teaching Assistant
          company: Rensselaer Polytechnic Institute
          company_url: 'https://www.rpi.edu/'
          company_logo: rpi
          location: Troy, NY, USA
          date_start: '2023-08-17'
          date_end: '2023-12-20'
          description: Taught basics of programming in Python to undergraduate students and conducted lab sessions.


        - title: Field Application Engineer
          company: NATS Inc.
          company_url: 'https://nats-usa.com/'
          company_logo: nats
          location: Dhaka, Bangladesh
          date_start: '2022-08-15'
          date_end: '2023-04-30'
          description: |2-
              Responsibilities include:

              * Installation, commissioning, and training of NATS products
              * Technical support to customers
              * Preparation of technical proposals



    design:
      columns: '2'
  - block: accomplishments
    content:
      # Note: `&shy;` is used to add a 'soft' hyphen in a long heading.
      title: 'Accomplish&shy;ments'
      subtitle:
      # Date format: https://docs.hugoblox.com/customization/#date-format
      date_format: Jan 2006
      # Accomplishments.
      #   Add/remove as many `item` blocks below as you like.
      #   `title`, `organization`, and `date_start` are the required parameters.
      #   Leave other parameters empty if not required.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - certificate_url: ''
          date_end: '2025-08-08'
          date_start: '2025-07-26'
          description: ''
          icon: 'argonne'
          organization: Argonne National Laboratory
          organization_url: https://extremecomputingtraining.anl.gov/
          title: Argonne Training Program on Extreme-Scale Computing (ATPESC) - 2025
          url: 'https://extremecomputingtraining.anl.gov/participants-2025/'

        - certificate_url: https://drive.google.com/file/d/1L6QXh2XoxzB-K5SIclCrPZG3b0usSnfB/view?usp=drive_link  
          date_end: '2022-05-23'
          date_start: '2022-05-23'
          description: ''
          icon: ictp
          organization: ICTP
          organization_url: https://www.ictp.it/
          title: Joint ICTP-IAEA Advanced School/Workshop on Computational Nuclear Science and Engineering
          url: ''

        - certificate_url: https://drive.google.com/file/d/1L6QXh2XoxzB-K5SIclCrPZG3b0usSnfB/view?usp=drive_link  
          date_end: '2022-09-17'
          date_start: '2022-09-13'
          description: ''
          icon: ictp
          organization: ICTP
          organization_url: https://www.ictp.it/
          title: Joint ICTP-IAEA Course on Theoretical Foundations and Application of Computational Fluid Dynamics in Nuclear Engineering
          url: ''
    design:
      columns: '2'
  - block: collection
    id: posts
    content:
      title: Recent Posts
      subtitle: ''
      text: ''
      # Choose how many pages you would like to display (0 = all pages)
      count: 0
      # Filter on criteria
      filters:
        folders:
          - post
        author: ""
        category: ""
        tag: ""
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ""
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: compact
      columns: '2'
  - block: portfolio
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - project
      # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
      default_button_index: 0
      # Filter toolbar (optional).
      # Add or remove as many filters (`filter_button` instances) as you like.
      # To show all items, set `tag` to "*".
      # To filter by a specific tag, set `tag` to an existing tag name.
      # To remove the toolbar, delete the entire `filter_button` block.
      buttons:
        - name: All
          tag: '*'
        - name: File I/O
          tag: File I/O
        - name: Other
          tag: Demo
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '1'
      view: showcase
      # For Showcase view, flip alternate rows?
      flip_alt_rows: false
  - block: markdown
    content:
      title: ''
      subtitle: ''
      text: |-
        <details>
          <summary style="cursor: pointer; text-align: center; margin-bottom: 2rem;">
            <span class="gallery-closed" style="font-size: 2.5rem; font-family: inherit;"><span style="font-size: 2.5rem; display: inline-block; width: 1em; text-align: center;">›</span> Gallery</span>
            <span class="gallery-open" style="display: none; font-size: 2.5rem; font-family: inherit;"><span style="font-size: 2.5rem; display: inline-block; width: 1em; text-align: center;">⌄</span> Gallery</span>
          </summary>
          <div style="margin-top: 1rem;">
            {{< gallery-with-captions album="hobbyprojects" >}}
          </div>
        </details>
        
        <style>
          details > summary {
            list-style: none;
          }
          details > summary::-webkit-details-marker {
            display: none;
          }
          details[open] > summary .gallery-closed {
            display: none;
          }
          details[open] > summary .gallery-open {
            display: inline !important;
          }
        </style>
    design:
      columns: '1'
  - block: collection
    id: featured
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
    design:
      columns: '2'
      view: card
  - block: collection
    content:
      title: Recent Publications
      text: |-
        {{% callout note %}}
        Quickly discover relevant content by [filtering publications](./publication/).
        {{% /callout %}}
      filters:
        folders:
          - publication
        exclude_featured: true
    design:
      columns: '2'
      view: citation
  - block: collection
    id: talks
    content:
      title: Recent & Upcoming Talks
      filters:
        folders:
          - event
    design:
      columns: '2'
      view: compact
  - block: contact
    id: contact
    content:
      title: Contact
      subtitle:
      email: fuadhhasan.for@gmail.com
      #phone: 888 888 88 88
      #appointment_url: 'https://calendly.com'
      address:
        street: CII, 15th Street
        city: Troy
        region: NY
        postcode: '12180'
        country: United States
        country_code: US
      directions: CII 7th Floor. Prof. Merson's Graduate Student Office
      #office_hours:
      #  - 'Monday 10:00 to 13:00'
      #  - 'Wednesday 09:00 to 10:00'
      # Choose a map provider in `params.yaml` to show a map from these coordinates
      coordinates:
        latitude: '42.7292887'
        longitude: '-73.6786747'  
      contact_links:
        - icon: video
          icon_pack: fas
          name: Webex Me
          link: 'https://rensselaer.webex.com/meet/hasanm4'
      # Automatically link email and phone or display as text?
      autolink: true
      # Email form provider
    design:
      columns: '2'
---
