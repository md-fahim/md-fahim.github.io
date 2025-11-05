[Title]
BanglaTLit: A benchmark dataset for back-transliteration of Romanized Bangla
[Image]
posts/publications/fruitquality.png
[Author]
Md Fahim, Fariha Shifat, Fabiha Haider, Deeparghya Barua, Md Sourove, Md Ishmam, Md Bhuiyan
[Paper]
[https://aclanthology.org/2024.findings-emnlp.859/](https://aclanthology.org/2024.findings-emnlp.859/)
[Code]
[https://github.com/farhanishmam/BanglaTLit](https://github.com/farhanishmam/BanglaTLit)
[BIB]
@inproceedings{fahim2024banglatlit,
  title={BanglaTLit: A benchmark dataset for back-transliteration of Romanized Bangla},
  author={Fahim, Md and Shifat, Fariha and Haider, Fabiha and Barua, Deeparghya and Sourove, Md and Ishmam, Md and Bhuiyan, Md},
  booktitle={Findings of the Association for Computational Linguistics: EMNLP 2024},
  pages={14656--14672},
  year={2024}
}
[Venue]
Findings of the Association for Computational Linguistics: EMNLP 2024
[Year]
2024

---

[Title]
BANMIME : Misogyny Detection with Metaphor Explanation on Bangla Memes
[Image]
posts/publications/fruitquality.png
[Author]
Md Ayon Mia*, Akm Moshiur Rahman*, Khadiza Sultana*, Md Fahim* [Project Lead], Md Tahmid Hasan, Muhammad Ibrahim Khan, AKM Mahbubur Rahman
[Paper]
[https://aclanthology.org/2025.emnlp-main.900/](https://aclanthology.org/2025.emnlp-main.900/)
[Code]
[https://github.com/Ayon128/BANMIME](https://github.com/Ayon128/BANMIME)
[BIB]
@inproceedings{mia-etal-2025-banmime,
    title = "{BANMIME} : Misogyny Detection with Metaphor Explanation on {B}angla Memes",
    author = "Mia, Md Ayon  and
      Mazumder, Akm Moshiur Rahman  and
      Sayma, Khadiza Sultana  and
      Fahim, Md  and
      Fuad, Md Tahmid Hasan  and
      Khan, Muhammad Ibrahim  and
      Rahman, Akmmahbubur",
    editor = "Christodoulopoulos, Christos  and
      Chakraborty, Tanmoy  and
      Rose, Carolyn  and
      Peng, Violet",
    booktitle = "Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing",
    month = nov,
    year = "2025",
    address = "Suzhou, China",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2025.emnlp-main.900/",
    pages = "17824--17850",
    ISBN = "979-8-89176-332-6",
    abstract = "Detecting misogyny in multimodal content remains a notable challenge, particularly in culturally conservative and low-resource contexts like Bangladesh. While existing research has explored hate speech and general meme classification, the nuanced identification of misogyny in Bangla memes, rich in metaphor, humor, and visual-textual interplay, remains severely underexplored. To address this gap, we introduce BanMiMe, the first comprehensive Bangla misogynistic meme dataset comprising 2,000 culturally grounded samples where each meme includes misogyny labels, humor categories, metaphor localization, and detailed human-written explanations. We benchmark the various performance of open and closed-source vision-language models (VLMs) under zero-shot and prompt-based settings and evaluate their capacity for both classification and explanation generation. Furthermore, we systematically explore multiple fine-tuning strategies, including standard, data-augmented, and Chain-of-Thought (CoT) supervision. Our results demonstrate that CoT-based fine-tuning consistently enhances model performance, both in terms of accuracy and in generating meaningful explanations. We envision BanMiMe as a foundational resource for advancing explainable multimodal moderation systems in low-resource and culturally sensitive settings."
}
[Venue]
Empirical Methods in Natural Language Processing (EMNLP 2025): Main Conference
[Year]
2025

---

[Title]
DM-Codec: Distilling Multimodal Representations for Speech Tokenization
[Image]
posts/publications/fruitquality.png
[Author]
Md Mubtasim, Md Fahim, Tasnim Mohiuddin, AKM Mahabubur Rahman, Aman Chadha, Tariq Iqbal, M Ashraful Amin, Md Mofijul Islam, Amin Ahsan Ali
[Paper]
[https://aclanthology.org/2025.findings-emnlp.1394/](https://aclanthology.org/2025.findings-emnlp.1394/)
[Code]
[https://github.com/mubtasimahasan/DM-Codec](https://github.com/mubtasimahasan/DM-Codec)
[BIB]
@inproceedings{ahasan-etal-2025-dm,
    title = "{DM}-Codec: Distilling Multimodal Representations for Speech Tokenization",
    author = "Ahasan, Md Mubtasim  and
      Fahim, Md  and
      Mohiuddin, Tasnim  and
      Rahman, Akmmahbubur  and
      Chadha, Aman  and
      Iqbal, Tariq  and
      Amin, M Ashraful  and
      Islam, Md Mofijul  and
      Ali, Amin Ahsan",
    editor = "Christodoulopoulos, Christos  and
      Chakraborty, Tanmoy  and
      Rose, Carolyn  and
      Peng, Violet",
    booktitle = "Findings of the Association for Computational Linguistics: EMNLP 2025",
    month = nov,
    year = "2025",
    address = "Suzhou, China",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2025.findings-emnlp.1394/",
    pages = "25580--25602",
    ISBN = "979-8-89176-335-7",
    abstract = "Recent advancements in speech-language models have yielded significant improvements in speech tokenization and synthesis. However, effectively mapping the complex, multidimensional attributes of speech into discrete tokens remains challenging. This process demands acoustic, semantic, and contextual information for precise speech representations. Existing speech representations generally fall into two categories: acoustic tokens from audio codecs and semantic tokens from speech self-supervised learning models. Although recent efforts have unified acoustic and semantic tokens for improved performance, they overlook the crucial role of contextual representation in comprehensive speech modeling. Our empirical investigations reveal that the absence of contextual representations results in elevated Word Error Rate (WER) and Word Information Lost (WIL) scores in speech transcriptions. To address these limitations, we propose two novel distillation approaches: (1) a language model (LM)-guided distillation method that incorporates contextual information, and (2) a combined LM and self-supervised speech model (SM)-guided distillation technique that effectively distills multimodal representations (acoustic, semantic, and contextual) into a comprehensive speech tokenizer, termed DM-Codec. The DM-Codec architecture adopts a streamlined encoder-decoder framework with a Residual Vector Quantizer (RVQ) and incorporates the LM and SM during the training process. Experiments show DM-Codec significantly outperforms state-of-the-art speech tokenization models, reducing WER by up to 13.46{\%}, WIL by 9.82{\%}, and improving speech quality by 5.84{\%} and intelligibility by 1.85{\%} on the LibriSpeech benchmark dataset."
}
[Venue]
Findings of the Association for Computational Linguistics: EMNLP 2025
[Year]
2025

---

[Title]
ChitroJera: A Regionally Relevant Visual Question Answering Dataset for Bangla
[Image]
posts/publications/fruitquality.png
[Author]
Deeparghya Dutta*, Md Sakib Ul Rahman*, Md Fahim*, Fabiha Haider, Fariha Tanjim, Md Farhan Ishmam, Farhad Alam
[Paper]
[https://arxiv.org/abs/2410.14991](https://arxiv.org/abs/2410.14991)
[Code]
[https://github.com/farhanishmam/ChitroJera](https://github.com/farhanishmam/ChitroJera)
[BIB]
@inproceedings{barua2025chitrojera,
  title={ChitroJera: A Regionally Relevant Visual Question Answering Dataset for Bangla},
  author={Barua, Deeparghya Dutta and Sourove, Md Sakib Ul Rahman and Fahim, Md and Haider, Fabiha and Shifat, Fariha Tanjim and Rahman Adib, Md Tasmim and Uddin, Anam Borhan and Ishmam, Md Farhan and Alam, Md Farhad},
  booktitle={Joint European Conference on Machine Learning and Knowledge Discovery in Databases},
  pages={473--491},
  year={2025},
  organization={Springer}
}
[Venue]
Joint European Conference on Machine Learning and Knowledge Discovery in Databases: ECML-PKDD 2025
[Year]
2025

---

