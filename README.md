# „Euros für Ärzte“ Data Analysis

[See Jupyter notebook.](awb_meldungen.ipynb)

## Steve's Notes


This analysis takes an interesting approach to entity resolution. The column `praeparat` is essentially a substring of the corresponding value of `Präparatname` as Python's `.split` method in the `clean_praeparat` function splits the string on characters that are not words or spaces, digits, and underscores. It takes the first item in the exploded string, strips it of whitespace, and reduces it to lowercase.

```
# Add simpler version of präparatname that might group better later

DRUG_NAME_SPLITTER = re.compile(r'[^\w ]|\d|_', re.U | re.I)

def clean_praeparat(praeparat):
    name = DRUG_NAME_SPLITTER.split(praeparat)[0].strip().lower()
    if len(name) < 4:
        return praeparat
    return name


df_all['praeparat'] = df_all[u'Präparatname'].apply(clean_praeparat)

print('Original Präparatname Number of Groups', len(df_all[u'Präparatname'].value_counts()))
print('Cleaned Präparatname Number of Groups', len(df_all['praeparat'].value_counts()))
```
