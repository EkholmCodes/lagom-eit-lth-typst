# Lagom EIT

**la·gom** - /*ˈlɑ̂ːɡɔm*/
adjective, adverb

Swedish word for not too little, not too much; just the right amount.

---

Unofficial thesis template made for degree projects at the institution of Electrical and Information technology (EIT) at Lund Faculty of Engineering (LTH), Lund University. Although being inspired by other templates and projects (for example the original template and the one from CS), this is an attempt to give students something fresh when looking to write their thesis in Typst. It is made to be easy to work with, having filled-out fields, premade folder structure and few extra functions. It still provides the ability for some customization. The front matter is not pre-determined as other templates and the front cover is customizable (as long as it complies with university standards).

It contains one template file which has four functions to call upon for the different documents in the degree project:

- **thesis** - Project report to be written.
- **popular-science-summary** - For promoting your work in a Lund University graphic based fashion.
- **goal-document** - For early planning and goal-setting for the project. 
- **project-plan** - Complements and is used inside the the goal document.

which all are available from startup as distinct documents.

## Usage

### To start

Open in the web app, or the CLI using

```bash
typst init @preview/lagom-eit-lth project
cd project
typst compile thesis.typ
```

### Package structure

The only thing present when initializing the project is the metadata and the three documents to be rendered, most notably the thesis and the popular science summary. These are filled out and can be previewed immidately, and contains enough information and examples to get started writing on ones own.

### Metadata

To consistently use the same parameters across all documents, the parameters are imported through ```metadata.typ```. This is done such that the writer only needs to bother once when configuring the documents. Not all fields are present here, for example the swedish translations for the titles, as those are not mandatory.

```typst
/* Metadata for the degree project */

#let title = [On the Importance of Modern Typesetting]
#let subtitle = [How and why some institutions never leave the guilded age]

#let authors = (
  (
      name: "John Doe",
      email: "john.doe@examplemail.com",
      civic-number: "010101-0101",
  ),
  (
      name: "Jane Doe",
      affiliation: "Lund University",
      email: "jane.doe@examplemail.com",
      civic-number: "020202-0202",  
  ),
)
 
#let supervisors = (
  academic: (
    name: "Academic Supervisor",
    email: "academic.supervisor@eit.lth.se",
    affiliation: "LTH"
  ),
  company: (
    name: "Company Supervisor",
    email: "company.supervisor@company.com",
    affiliation: "Company name"
  ),
  company2: (
    name: "A second supervisor",
    email: "company.supervisor2@company.com",
    affiliation: "Company"
  )
)

#let examiner = (
  name: "Examiner",
  email: "academic.examiner@eit.lth.se"
)

#let affiliation = (
  (
    name: "Some company",
    logo: none
  ),
)

#let keywords = ("Keyword 1", "Keyword 2")
#let start-date = datetime(year: 2004, month: 3, day: 21)
#let end-date = datetime(year: 2022, month: 10, day: 22)
```

## thesis

### Configuration

#### Document Fields

| Parameter | Type | Required / Default | Description |
| :--- | :--- | :--- | :--- |
| `thesis-title` | `string` / `content` | **Required** | The main title of the thesis. |
| `thesis-title-sv` | `string` / `content` | `none` | Swedish translation of the thesis title. |
| `thesis-subtitle` | `string` / `content` | `none` | Optional subtitle for the thesis. |
| `thesis-subtitle-sv` | `string` / `content` | `none` | Swedish translation of the subtitle. |
| `short-title` | `content` / `string` | `[#degree-level-en's thesis]` | Shortened title used in page headers. |
| `authors` | `array` | **Required** | List of authors. Expected structure:<br>`((name: "", email: ""),)` |
| `supervisors` | `dictionary` | **Required** | Dictionary containing supervisor details. Expected keys/structure:<br>`academic: (name: "", email: "", affiliation: "")`<br>`company: (name: "", email: "", affiliation: "")` |
| `examiner` | `dictionary` | **Required** | Dictionary containing examiner details. Expected structure:<br>`(name: "", email: "")` |
| `course-code` | `content` / `string` | `[EITM01]` | Course code for the thesis module. |
| `affiliations` | `array` / `dictionary` | `none` | Optional affiliations to the degree project (host compoanies for example). Expected structure: <br>`((name: "", logo: path("")),)` |
| `description` | `string` / `content` | `none` | Optional brief summary or metadata description. |
| `keywords` | `array` | `()` | List of keywords for document metadata. |
| `document-style` | `string` | `"revised"` | Layout style for headers/headings. Allowed values: `"original"` or `"revised"`. |
| `front-cover-background` | `content` or `none` | `rect(width: 100%, height: 100%, fill: lu-light-brown)` | Visual element or color definition for the cover background. If set to none, no front and back covers are generated. |
| `date` | `datetime` | `datetime.today()` | Publication or submission date. |
| `report-number` | `string` / `integer` | `none` | Report ID number. **Required if `print` is set to `true`**. |
| `issn` | `string` | `none` | ISSN identifier for published prints. |
| `print` | `bool` | `false` | Enables print mode formatting (requires `report-number`). |


### ```print``` field

When this parameter is set to true, the thesis will be put to a G5 format with front and end covers. The front cover is governed by ```front-cover-background```. To print, the thesis must have a report number, which is given by the institution. By default print is set to false, which renders the report on an a4 paper with some metadata on the top along with bounding boxes illustrating the G5 paper, which is used for drafting. If the author wished to generate a print-ready document without covers, ```front-cover-background``` can be set to ```none```.

### state functions

To enter a specific style (or state) inside the thesis, the template contains three functions for the three types of matter to be used. ```frontmatter```, ```mainmatter```, and ```backmatter```. These define how counters, page numbering and various other things work and are called using show-rules. See the example for usage of the functions.

### ```flex-caption``` function

This function gives the ability to use a different body in headings and figure captions in the document and compared to the outline, for example when having a longer and more explanatory caption or a heading spanning several lines.

### Example

```typst
#import "@preview/lagom-eit-lth:0.1.0": thesis, mainmatter, frontmatter, backmatter, flex-caption
#import "metadata.typ": *

#show: thesis.with(
  thesis-title: title,
  thesis-subtitle: subtitle,
  authors: authors,
  supervisors: supervisors,
  examiner: examiner,
  affiliations: affiliation,
  keywords: keywords,
  description: "Unofficial thesis template for degree projects at Electrical and information technology at Lund University.",
  date: end-date,
  issn: none,
  report-number: none,
  print: false,
)

#show: frontmatter

// Here goes your frontmatter

#show: mainmatter

// Here goes your mainmatter

#show: backmatter

// Here goes your appendicies

```


## popular-science-summary

### Document fields
| Field | Type | Required / Default | Description |
| :--- | :--- | :--- | :--- |
| `summary-title` | `string` / `content` | **Required** | The title of the thesis summary. |
| `original-title` | `string` / `content` | **Required** | The original title of the full thesis. |
| `authors` | `array` | **Required** | List of author names/details (e.g., `("Author Name",)`). |
| `supervisors` | `dictionary` | **Required** | Dictionary containing supervisor details. Expected keys/structure:<br>`academic: (name: "", email: "", affiliation: "")`<br>`company: (name: "", email: "", affiliation: "")` |
| `examiner` | `dictionary` | **Required** | Dictionary containing examiner details. Expected keys/structure:<br>`(name: "", email: "")` |
| `lead-paragraph` | `string` / `content` | `none` | Optional introductory summary/lead text. |
| `thesis-link` | `string` | `none` | Optional link to the full thesis document/repository. |
| `presentation-date` | `datetime` | `datetime.today()` | The date of the presentation. Defaults to current date. |
| `lang` | `string` | `"sv"` | Language setting for the document. Allowed values: `"sv"` or `"en"`. |

## goal-document

### Document fields

| Parameter | Type | Required / Default | Description |
| :--- | :--- | :--- | :--- |
| `tentative-title` | `string` / `content` | **Required** | The working or tentative title for the thesis proposal/project. |
| `authors` | `array` | **Required** | List of authors or student details. |
| `start-date` | `datetime` | **Required** | Start date of the thesis project. |
| `end-date` | `datetime` | **Required** | End date or planned completion date of the project. |
| `course-code` | `string` / `content` | **Required** | Course code for the thesis module. |
| `academic-supervisor` | `string` / `dictionary` | **Required** | Details or name of the assigned academic supervisor. |
| `examiner` | `string` / `dictionary` | **Required** | Details or name of the assigned examiner. |
| `lang` | `string` | `"en"` | Language setting for the document. Allowed values: `"en"` or `"sv"`. |

## project-plan

### Document fields

| Parameter | Type | Required / Default | Description |
| :--- | :--- | :--- | :--- |
| `academic-supervisor` | `string` / `dictionary` | **Required** | Details or name of the assigned academic supervisor. |
| `examiner` | `string` / `dictionary` | **Required** | Details or name of the assigned examiner. |
| `lang` | `string` | `"en"` | Language setting for the document. Allowed values: `"en"` or `"sv"`. |

## Todo

- ~~Fix image in affiliation to be of type content instead of string to the image path.~~ (It is now of type path which is supported since Typst 0.15.0)
- ~~Separate styling to a different file called style.typ?~~ (Several new files for easier workflow)
- ~~Fix figure and table counters.~~ (Now numbers as IEEE standard, for example Figure 1.1 and Table 2.1)
- Finish project plan template.
- Confirm with local printing that front and back covers are compliant.
- ~~Have the option to skip front and back cover but still print as G5.~~ (Put ```front-cover-background``` to none)