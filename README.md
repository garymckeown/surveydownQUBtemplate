# Queen's University Belfast surveydown Template

This is a minimal template for the [surveydown](https://www.surveydown.org) R package to allow the creation of surveys and online experiments that are based on Queen's University Belfast branding and requirements.

The template includes a Participant Information Sheet and Consent form template that are provided as exemplars by the [Faculty of Engineering and Physical Sciences Research Ethics Committee](https://www.qub.ac.uk/Research/Governance-ethics-and-integrity/Ethics/FacultyResearchEthicsCommittees/) (EPS REC).

The Participant Information Sheet needs to be adjusted to suit the needs of the current study.

Some of the questions on the consent form have been commented out, but have been left in the document should people choose to use them the comments should be removed and adjustments should be made as necessary.

Two demographic questions have been added in an "About You" page, these ask for age using a numeric question and ask for gender using the preferred question structure as recommended by the EPS REC.

There are meta data variables included in the yml of the survey.qmd file these are currently

-   version: 1

-   date: "27/10/2025"

-   chief_investigator: "Gary McKeown"

-   project_title: "Our Project Title"

-   study_number: "EPS 25_170"

These can be populated as desired by the research team and they will automatically populate in the Participant Information Sheet and the Consent form. They can be added to any page of the document using the shortcode:

{{< meta params.version >}}

The idea here is to place the once in the yml to avoid human error from having to correct this multiple times in the file.

To use this template you can either fork the GitHub repository or download it and use it as a basic RStudio project.
