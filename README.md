# Text-Generation-with-Markov-Chains

### Overview

This project demonstrates the implementation of a simple text generation algorithm using Markov chains. The source text used is "Frankenstein" by Mary Shelley. The text generation is further enhanced by incorporating part-of-speech (POS) tagging to produce more coherent sentences.

### Features

- **PDF Text Extraction**: Extracts text from a PDF file using the `PyPDF2` library.
- **Text Cleaning**: Cleans the extracted text by removing unwanted sections and formatting.
- **Markov Chain Text Generation**: Implements text generation using the `markovify` library.
- **Enhanced Text Generation with POS Tagging**: Utilizes `spaCy` for POS tagging to generate more legible text.

### Libraries Used

- **PyPDF2**: For extracting text from PDF files.
- **markovify**: For building Markov chain-based text generators.
- **spaCy**: For POS tagging to improve text generation.
- **re**: For text cleaning with regular expressions.
- **pandas & numpy**: For data manipulation and numerical operations (although not used extensively in this example).

### How to Use

1. **Extract Text from PDF**:
   ```python
   def read_pdf(file_path):
       with open(file_path, "rb") as file:
           pdf_reader = PdfReader(file)
           text = ""
           for page_num in range(len(pdf_reader.pages)):
               text += pdf_reader.pages[page_num].extract_text()
       return text
   ```

2. **Clean Extracted Text**:
   ```python
   def clean_text(text, book_title):
       text = re.sub(r'LETTER\s\d+', '', text)
       text = text.replace("Frankenstein ", '\n')
       text = text.replace(" Free eBooks at Planet eBook.", '\n')
       text = re.sub(r'\n+', '\n', text).strip()
       return text
   ```

3. **Generate Text Using Markov Chains**:
   ```python
   generator_1 = markovify.Text(input_text, state_size=3)
   generated_text1 = generator_1.make_sentence()
   generated_text2 = generator_1.make_short_sentence(max_chars=512)
   ```

4. **Enhanced Text Generation with POS Tagging**:
   ```python
   class POSifiedText(markovify.Text):
       def word_split(self, sentence):
           return ['::'.join((word.orth_, word.pos_)) for word in nlp(sentence)]
       def word_join(self, words):
           sentence = ' '.join(word.split('::')[0] for word in words)
           return sentence

   generator_2 = POSifiedText(input_text, state_size=3)
   generated_text3 = generator_2.make_sentence()
   generated_text4 = generator_2.make_short_sentence(max_chars=512)
   ```

### Results

- **Without POS Tagging**:
  ```python
  print(generated_text2)
  ```

- **With POS Tagging**:
  ```python
  print(generated_text4)
  ```

### Conclusion

This project provides a straightforward implementation of text generation using Markov chains, enhanced by POS tagging to create more coherent and grammatically correct sentences. It serves as an educational tool for understanding the basics of Markov chain-based text generation and the benefits of incorporating linguistic features like POS tags.

### Files

- `Frankenstein by Mary Shelley.pdf`: The source text for generation.
- `Cleaned_Frankenstein by Mary Shelley.txt`: The cleaned text used for training the Markov model.
- `text_generation.py`: The main script for text extraction, cleaning, and generation.

