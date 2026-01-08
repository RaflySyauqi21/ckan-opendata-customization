## ENGLISH ##

## Feature: Dataset Visibility Control
This customization restricts dataset visibility changes to sysadmin users only.

### Purpose
- Sysadmin can set dataset visibility (public/private)
- Non-sysadmin datasets are automatically saved as private
- Backend still receives `private=True` to ensure consistency

### Modified file
- ckan/templates/package/snippets/package_basic_fields.html

### Patch
- patches/feature-visibility-dataset.diff

## INDONESIAN ##

## Fitur: Kontrol Visibilitas Dataset
Kustomisasi ini membatasi perubahan visibilitas dataset hanya untuk pengguna sysadmin.

### Tujuan
- Sysadmin dapat mengatur visibilitas dataset (publik/pribadi)
- Dataset non-sysadmin secara otomatis disimpan sebagai pribadi
- Backend tetap menerima `private=True` untuk memastikan konsistensi

### File yang Dimodifikasi
- ckan/templates/package/snippets/package_basic_fields.html

### Patch
- patches/feature-visibility-dataset.diff
