# Textures

Ce dossier est vide dans le dépôt : les mosaïques LROC pèsent plusieurs gigaoctets et ne sont pas
versionnées. L'application démarre sans elles (le shader retombe sur ses valeurs par défaut, la
Lune s'affiche en gris uniforme éclairé).

## Fichiers attendus

| Fichier | Source | Traitement |
|---|---|---|
| `moon-color-{preview,medium,high,ultra}.webp` | LROC WAC Global Mosaic 100 m/px (NASA/GSFC/ASU) | équirectangulaire, réduit en 512 / 2048 / 8192 / 16384 px |
| `moon-normal-*.webp` | dérivée de LOLA + SELENE (SLDEM2015) | normal map tangent-space |
| `moon-displacement-*.webp` | SLDEM2015 | hauteur 16 bits encodée sur les canaux R et G |
| `moon-ao-*.webp` | occlusion ambiante précalculée depuis le MNT | canal R |
| `moon-roughness-*.webp` | dérivée de l'albédo Hapke | canal G |

## Récupération

Domaine public (NASA / USGS Astrogeology) :
<https://astrogeology.usgs.gov/search/map/moon_lro_lroc_wac_global_morphology_mosaic_100m>

```bash
magick lroc_wac_global.tif -resize 16384x8192 -quality 88 moon-color-ultra.webp
magick lroc_wac_global.tif -resize 8192x4096  -quality 86 moon-color-high.webp
magick lroc_wac_global.tif -resize 2048x1024  -quality 82 moon-color-medium.webp
magick lroc_wac_global.tif -resize 512x256    -quality 75 moon-color-preview.webp
```

En production, préférez KTX2 / Basis Universal (compression GPU, ~6× moins de VRAM) :

```bash
basisu -uastc -ktx2 -y_flip moon-color-ultra.png
```
