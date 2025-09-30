flowchart TD
    A1[1. ФОРМА (HTML)\n<input name="collection_basis" value="Абзац другий..."/>] 
    -->|POST /dataset/new| A2[2. ВАЛІДАТОР (validators.py)\nua_collection_basis_validator(value, context)\n✓ Перевірка довжини: 10-500 символів]

    A2 -->|Валідація OK| A3[3. СКАН СХЕМА (schema.py)\nВизначає, що 'collection_basis' → extras]

    A3 --> A4[4. БАЗА ДАНИХ (PostgreSQL)\nINSERT INTO package_extra\n(id, package_id, key, value)\nVALUES('uuid','pkg-id','collection_basis','Абзац...')]

    A4 --> A5[5. ЗАВАНТАЖЕННЯ (package_show)\nSELECT p.*, pe.key, pe.value FROM package p\nLEFT JOIN package_extra pe ON p.id = pe.package_id]

    A5 --> A6[6. DICTIZE (model_dictize.py)\npkg_dict = {'collection_basis': 'Абзац другий...'}\nExtras додаються як поля верхнього рівня!]

    A6 --> A7[7. ШАБЛОН (additional_info.html)\n{{ h.ua_get_dataset_field_value(pkg_dict,'collection_basis') }}]

    A7 --> A8[8. HELPER (ui.py)\nvalue = pkg_dict.get('collection_basis')\nreturn value or default]

    A8 --> A9[9. HTML (відображення)\n<dd>Абзац другий пункту 14 Положення...</dd>]
