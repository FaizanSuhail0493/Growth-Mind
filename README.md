# Data Sweeper

## Overview
Data Sweeper is a Streamlit-based web application that allows users to upload CSV or Excel files, perform data cleaning, visualize data, and convert files between CSV and Excel formats.

## Features
- Upload multiple CSV or Excel files
- Preview uploaded data
- Perform data cleaning (remove duplicates, fill missing values)
- Select specific columns for conversion
- Visualize numerical data
- Convert files between CSV and Excel formats
- Download processed files

## Installation
To run the application locally, follow these steps:

```bash
# Clone the repository
git clone https://github.com/yourusername/datasweeper.git

# Navigate to the project directory
cd datasweeper

# Install required dependencies
pip install streamlit pandas openpyxl

# Run the Streamlit app
streamlit run app.py
```

## Functions and Their Usage
### 1. **`st.set_page_config()`**
Configures the Streamlit app layout and appearance.
- `page_title="Data Sweeper"`: Sets the title of the web page.
- `layout="wide"`: Uses a wide layout for better visualization.

### 2. **Custom CSS Styling (`st.markdown()`)**
Applies custom styles for dark mode, button styling, and table formatting using HTML and CSS.

### 3. **`st.title()` and `st.write()`**
- `st.title("Advanced Data Sweeper By Faizan Suhail")`: Displays the main application title.
- `st.write()`: Provides an introductory description of the application.

### 4. **File Upload (`st.file_uploader()`)**
- Accepts multiple files with extensions `.csv` and `.xlsx`.
- `accept_multiple_files=True`: Enables batch uploading of multiple files.

### 5. **File Processing**
- Extracts file extension using `os.path.splitext(file.name)`.
- Reads the uploaded file into a Pandas DataFrame:
  - `pd.read_csv(file)`: Reads CSV files.
  - `pd.read_excel(file)`: Reads Excel files.

### 6. **Displaying File Information**
- `st.write(f"📄 File Name: {file.name}")`: Shows file name.
- `st.write(f"📏 File Size: {file.size / 1024:.2f} KB")`: Displays file size in KB.

### 7. **Data Preview (`st.dataframe()`)**
- `st.dataframe(df.head())`: Displays the first five rows of the uploaded file.

### 8. **Data Cleaning Options**
- `st.subheader("🛠️ Data Cleaning Options")`: Creates a section for cleaning.
- `st.checkbox()`: Checkbox to enable cleaning features.
- Removing duplicates:
  ```python
  if st.button(f"Remove Duplicates from {file.name}"):
      df.drop_duplicates(inplace=True)
      st.write("Duplicates Removed!")
  ```
- Filling missing numeric values:
  ```python
  if st.button(f"Fill Missing Values for {file.name}"):
      numeric_cols = df.select_dtypes(include=['number']).columns
      df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].mean())
      st.write("Missing Values in Numeric Columns Filled!")
  ```

### 9. **Column Selection (`st.multiselect()`)**
- Allows users to choose specific columns for conversion:
  ```python
  columns = st.multiselect(f"Choose Columns for {file.name}", df.columns, default=df.columns)
  df = df[columns]
  ```

### 10. **Data Visualization (`st.bar_chart()`)**
- Plots the first two numeric columns as a bar chart:
  ```python
  if st.checkbox(f"Show Visualization for {file.name}"):
      st.bar_chart(df.select_dtypes(include='number').iloc[:, :2])
  ```

### 11. **File Conversion (`st.radio()`, `st.button()`, `st.download_button()`)**
- Users choose between CSV and Excel conversion:
  ```python
  conversion_type = st.radio(f"Convert {file.name} to:", ["CSV", "Excel"], key=file.name)
  ```
- Converts and stores the file in memory using `BytesIO()`:
  ```python
  buffer = BytesIO()
  if conversion_type == "CSV":
      df.to_csv(buffer, index=False)
      file_name = file.name.replace(file_extension, ".csv")
      mime_type = "text/csv"
  elif conversion_type == "Excel":
      df.to_excel(buffer, index=False, engine='openpyxl')
      file_name = file.name.replace(file_extension, ".xlsx")
      mime_type = "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  buffer.seek(0)
  ```
- Provides a download button for the converted file:
  ```python
  st.download_button(
      label=f"⬇️ Download {file.name} as {conversion_type}",
      data=buffer,
      file_name=file_name,
      mime=mime_type
  )
  ```

### 12. **Success Message (`st.success()`)**
- Displays a message once all files are processed:
  ```python
  st.success("🎉 All files processed successfully!")
  ```

## Usage
1. Upload a CSV or Excel file.
2. View file details and preview the data.
3. Choose cleaning options (remove duplicates, fill missing values).
4. Select specific columns to process.
5. View data visualization.
6. Convert the file to CSV or Excel.
7. Download the processed file.

## License
This project is licensed under the MIT License.

## Author
Developed by **Faizan Suhail**

