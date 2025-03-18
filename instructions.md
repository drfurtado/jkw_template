# Table Formatting Instructions for Quarto Academic Articles

This document provides instructions for creating properly formatted tables in Quarto academic articles, specifically for the APA7 template with two-column layout.

# Website documentation for this repo

visit this site before implementing anything:

https://wjschne.github.io/apaquarto/options.html

# Issues with this repo and fixes

https://github.com/wjschne/apaquarto/issues/38

## Table Types

There are two main types of tables you might need in an academic article:

1. **Single-Column Table**: Fits within one column of a two-column layout
2. **Full-Width Table**: Spans across both columns of a two-column layout

## 1. Single-Column Table

Use this approach when your table has fewer columns or simpler data that can fit within a single column width.

### Implementation

```markdown
```{=latex}
\begin{table}
\caption{Demographic Characteristics of Participants by Experimental Condition}
\begin{tabular}{@{}lccc@{}}
\hline
Characteristic & Ctrl & Int. A & Int. B \\
\hline
$n$ & 42 & 38 & 40 \\
Age (yrs) & 24.6 (3.8) & 25.2 (4.1) & 23.9 (3.5) \\
Gender (\%) & & & \\
\quad Female & 57.1 & 55.3 & 60.0 \\
\quad Male & 42.9 & 44.7 & 40.0 \\
BMI & 23.8 (2.9) & 24.1 (3.2) & 23.5 (2.7) \\
PA & 156.3 (45.7) & 162.8 (52.3) & 149.5 (48.6) \\
\hline
\end{tabular}

\vspace{0.5em}
\begin{tablenotes}[flushleft]
\footnotesize
\textit{Note.} Values are mean (SD). BMI = body mass index (kg/m²); PA = physical activity (min/week); Ctrl = Control; Int. = Intervention.
\end{tablenotes}
\end{table}
```

### Key Elements

- **`@{}` at the beginning and end of column specification**: Removes extra padding at the edges
- **`\begin{tablenotes}[flushleft]`**: Ensures notes align with the left edge of the table
- **`\vspace{0.5em}`**: Adds vertical space between the table and notes
- **Column specification (`lccc`)**: Defines left-aligned text column followed by three centered columns

## 2. Full-Width Table

Use this approach for tables with many columns or complex data that need to span the entire page width.

### Implementation

```markdown
```{=latex}
\begin{table*}[t]
\caption{Comparison of Intervention Effects Across Multiple Outcome Measures}
\begin{tabular*}{\textwidth}{@{\extracolsep{\fill}}lccccc@{}}
\hline
\multirow{2}{*}{Outcome Measure} & \multicolumn{3}{c}{Mean Change from Baseline (95\% CI)} & \multicolumn{2}{c}{Between-Group Differences} \\
\cline{2-6}
 & Control Group & Intervention A & Intervention B & A vs. Control & B vs. Control \\
\hline
Physical activity (min/week) & 12.4 (5.3, 19.5) & 45.7 (38.2, 53.2) & 52.3 (44.1, 60.5) & 33.3* & 39.9* \\
BMI (kg/m$^2$) & -0.2 (-0.5, 0.1) & -1.1 (-1.4, -0.8) & -1.3 (-1.6, -1.0) & -0.9* & -1.1* \\
Systolic BP (mmHg) & -2.1 (-4.3, 0.1) & -8.5 (-10.7, -6.3) & -9.2 (-11.5, -6.9) & -6.4* & -7.1* \\
Diastolic BP (mmHg) & -1.5 (-3.0, 0.0) & -5.2 (-6.7, -3.7) & -5.8 (-7.3, -4.3) & -3.7* & -4.3* \\
VO$_2$ max (mL/kg/min) & 0.8 (0.1, 1.5) & 3.2 (2.5, 3.9) & 3.9 (3.2, 4.6) & 2.4* & 3.1* \\
Quality of life score & 2.3 (0.5, 4.1) & 8.7 (6.9, 10.5) & 9.5 (7.7, 11.3) & 6.4* & 7.2* \\
\hline
\end{tabular*}

\vspace{0.5em}
\begin{tablenotes}[flushleft]
\footnotesize
\textit{Note.} CI = confidence interval; BMI = body mass index; BP = blood pressure; VO$_2$ max = maximal oxygen consumption. \\
* $p < .001$ for between-group difference.
\end{tablenotes}
\end{table*}
```

### Key Elements

- **`\begin{table*}[t]`**: The asterisk (*) makes the table span both columns; the `[t]` option positions it at the top of a page
- **`\begin{tabular*}{\textwidth}`**: Sets the table width to the full text width
- **`@{\extracolsep{\fill}}`**: Distributes extra space evenly between columns
- **`\multirow{2}{*}{...}`**: For cells that span multiple rows
- **`\multicolumn{3}{c}{...}`**: For cells that span multiple columns
- **`\cline{2-6}`**: Creates a horizontal line across specified columns only

## 3. Section Heading Spacing

When using LaTeX section commands in Quarto documents, you may notice excessive spacing after section headings. This can be fixed by modifying the section command parameters in the document preamble.

### Implementation

Add the following to your YAML frontmatter:

```yaml
format:
  apaquarto-pdf:
    include-in-header: 
      text: |
        \usepackage{etoolbox}
        \makeatletter
        \patchcmd{\section}
          {3.5ex \@plus 1ex \@minus .2ex}
          {1.5ex \@plus 1ex \@minus .2ex}
          {}{}
        \makeatother
```

### Key Elements

- **`include-in-header`**: Allows adding custom LaTeX code to the document preamble
- **`\usepackage{etoolbox}`**: Provides the `\patchcmd` command for modifying LaTeX commands
- **`\patchcmd{\section}`**: Modifies the section command to reduce spacing
- **`{3.5ex \@plus 1ex \@minus .2ex}`**: The original spacing before section headings
- **`{1.5ex \@plus 1ex \@minus .2ex}`**: The reduced spacing (adjust this value as needed)

This approach is preferred over adding `\vspace{-1.5em}` after each section heading because:

1. It applies consistently to all section headings
2. It modifies the spacing at the source rather than compensating after the fact
3. It's easier to maintain and adjust if needed

## Important Tips

1. **Text Flow**: Always include explanatory text before and after full-width tables to maintain narrative flow.

2. **Section Headings**: If you encounter compilation errors with markdown headings (#), use LaTeX section commands instead:
   ```latex
   ```{=latex}
   \section{Discussion}
   ```
   ```

3. **Table Placement**: LaTeX will try to place tables optimally, but you can influence placement with options:
   - `[t]`: Top of page
   - `[b]`: Bottom of page
   - `[h]`: Here (approximately where it appears in the code)
   - `[p]`: On a page containing only floats

4. **Table Notes**: Always use the `tablenotes` environment with the `flushleft` option to ensure proper alignment.

5. **Spacing**: Use `\vspace{0.5em}` to add appropriate vertical space between the table and notes.

6. **Column Width**: For tables with long text in the first column, you can specify a fixed width:
   ```latex
   \begin{tabular}{p{2.2cm}ccc}
   ```

7. **Math Symbols**: Use `$...$` for inline math expressions (e.g., `$p < .001$`).

8. **Superscripts**: Use `$^2$` for superscripts (e.g., kg/m$^2$).

## Troubleshooting

- If tables appear too wide for a column, try:
  - Using abbreviations in headers
  - Reducing column padding with `\setlength{\tabcolsep}{4pt}`
  - Using smaller font size with `\small` or `\footnotesize`

- If you get "overfull hbox" warnings:
  - Check if your table is too wide
  - Try using `\begin{adjustbox}{width=\columnwidth}` around your tabular environment

- If headings with `#` cause compilation errors:
  - Replace markdown headings with LaTeX `\section{}` commands
