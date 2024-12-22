---
title: "Development of a Serial Number Extractor for Nigerian Currencies"
authors: 
  - LB Adewole
  - EO Ogunleye
  - admin
  - SA Adewale
  - CY Daramola
date: "2023-08-02T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2023-08-02T00:00:00Z"

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ["journal"]

# Publication name and optional abbreviated publication name.
publication: ""
publication_short: ""

abstract: "The need to track currency movement and validate currencies in circulation are two cogent reasons for developing a real-time currency serial number extractor. Due to significant technical innovation over the past few decades, currency counterfeiting issues have gotten progressively worse all over the world and it is presently one of the main issues in Nigeria. Hence, financial institutions and the general public often desire to know the authenticity of cash. Also, Government and security agencies often desire to track cash for the purpose of apprehending notorious kidnappers after ransom collection. This calls for a fast and accurate serial number extraction from currencies. Automatic serial number extraction can be segmented into three (3) phases, which include currency classification, region of interest extraction, and character recognition. In this paper, an optimized object identification model was proposed for currency classification. A pre-trained denomination-based region of interest algorithm was then applied for the extraction of the serial number region. We further utilized three (3) existing optical character recognition models: Pytesseract, Easy OCR, and Keras OCR for the character recognition. Accuracy, Levenshtein Distance, Jaccard Similarity, Character Error Rate, and Damerau Distance metrics were employed in this study for recognition performance measure. The currency bill classification yielded a 100% accuracy while the best accuracy of 81% was obtained with the EasyOCR framework at the character recognition phase. The recognition model performance can be considered poor for the targeted application. Hence, the need for customization of the general character recognition architecture for Nigerian currency bill serial number recognition."

summary: "
This study develops a real-time currency serial number extractor to address counterfeiting and cash tracking in Nigeria. It uses currency classification, region extraction, and character recognition with OCR models. While achieving 100% classification accuracy, the best OCR accuracy was 81%, highlighting the need for customized recognition models for Nigerian currency."

tags:
  - Currency Serial Number Recognition
  - Counterfeit Detection
  - Optical Character Recognition 
featured: true

links:
  - name: Link
    url: http://journals.ui.edu.ng/index.php/uijslictr/article/view/1142
url_pdf: http://journals.ui.edu.ng/index.php/uijslictr/article/view/1142/960
url_code: https://github.com/Theedon/NairaSerialNumberExtractor"
# url_dataset: "#"
# url_poster: "#"
# url_project: ""
# url_slides: ""
# url_source: "#"
# url_video: "#"

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: "featured image"
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
  - internal-project

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---

<!-- This work is driven by the results in my [previous paper](/publication/conference-paper/) on LLMs.

{{% callout note %}}
Create your slides in Markdown - click the _Slides_ button to check out the example.
{{% /callout %}}

Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->
