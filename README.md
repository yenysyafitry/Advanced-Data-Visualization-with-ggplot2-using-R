<p align="justify"> Bahasa R memiliki berbagai sistem untuk membuat grafik, baik grafik statis maupun grafik interaktif. Pada grafik statis misalnya terdapat sistem base, lattice, ggplot, dan lainnya. Di dalam modul ini Anda akan diajak untuk mengenal salah satu sistem grafik yang paling populer di R, yaitu ggplot.
Sistem pembuatan grafik dengan ggplot dapat dilakukan dengan menggunakan paket ggplot2 yang merupakan implementasi dari konsep Grammar of Graphic untuk bahasa pemrograman R. Pada prinsipnya, konsep Grammar of Graphic mengajak kita untuk merekonstruksi pembuatan grafik dengan menggunakan kaidah tata bahasa sehingga tidak terikat pada nama jenis grafik (contoh: scatterplot, line-chart, bar-chart, dll.) </p>
<p align="justify"> <b>Konsep Grammar of Graphic</b></br>library(ggplot2)</p>
<p align="justify"> <b>Mengingat Kembali</b></br>library(ggplot2)</br>
qplot(x = carat, y = price, colour = clarity, data = diamonds)</br><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download.png"></p>
<p align="justify"> <b>Pembuatan Grafik dengan ggplot</b></br>library(ggplot2) </br> ggplot(data = diamonds, mapping = aes(x = carat, y = price, colour = clarity)) +
  geom_point()</br><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (1).png"></p>
<p align="justify"> <b>Kode 3 Rupa</b></br>library(ggplot2)diamonds_c1 <- ggplot( data = diamonds, mapping = aes(x = carat, y = price, colour = clarity)) + geom_point()
summary(diamonds_c1)</br>
diamonds_c2 <- ggplot(data = diamonds) + geom_point(mapping = aes(x = carat, y = price, colour = clarity))</br>
summary(diamonds_c2)</br>
diamonds_c3 <- ggplot() + geom_point( data = diamonds,mapping = aes(x = carat, y = price, colour = clarity))</br>
summary(diamonds_c3)</p>
<p align="justify"> <b>Yin dan Yang</b></br>library(dplyr)	</p>
<b>Paket dplyr</b></br>Terdapat beberapa fungsi utama dari paket dplyr untuk melakukan transformasi data, diantaranya:
<ol>
<li>select()</li>
<li>filter()</li>
<li>arrange()</li>
<li>mutate()</li>
<li>summarise()</li>
<li>group_by()	</li></ol>
<table>Paket dplyr</b></td> <td><i>TRUE</i></table>
<p align="justify"> <b>Transformasi Data</b></br>library(dplyr)</br>
glimpse(storms)</br>
# Tanpa menggunakan %>%</br>
storms1 <- select(storms, year, month, wind, pressure)</br>
storms2 <- filter(storms1, between(year, 2000, 2015))</br>
storms3 <- mutate(storms2, month = factor(month.name[storms2$month], levels = month.name))</br>
storms4 <- group_by(storms3, month)</br>
storms_nopipe <- summarise(storms4, avg_wind = mean(wind), avg_pressure = mean(pressure))</br>
glimpse(storms_nopipe)</br>
# Menggunakan %>%</br>
storms_pipe <- storms %>% </br>
select(year, month, wind, pressure) %>%</br>
filter(between(year, 2000, 2015)) %>%</br>
mutate(month = factor(month.name[month], levels = month.name)) %>%</br>
group_by(month) %>%</br>
summarise(avg_wind = mean(wind), avg_pressure = mean(pressure))</br>
glimpse(storms_pipe)</br>
# Komparasi metode tanpa pipe dan dengan pipe</br>
identical(storms_nopipe, storms_pipe) </p>

 <b>Import Dataset</b><details> <summary>> library(readr)</br> 
indodapoer <- read_tsv("https://storage.googleapis.com/dqlab-dataset/indodapoer.tsv.gz")</br> 
nrow(indodapoer)</br> 
ncol(indodapoer) </summary><table><tr><td> > nrow(indodapoer)</br>
[1] 22468 </br> > ncol(indodapoer)</br> 
[1] 222</td></tr></table></details>


 <b>Wild Names and How to Tame Them</b><details> <summary>> install.packages("janitor", repos = "http://cran.us.r-project.org")</br>
library(janitor)</br>
head(colnames(indodapoer), 15)</br>
indodapoer <- clean_names(indodapoer)</br>
head(colnames(indodapoer), 15)</summary><table><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_1.jpg"></table></details>

 <b>Produk Domestik Regional Bruto</b><details> <summary>>library(stringr)</br>
library(dplyr)</br>
pdrb_pjawa <- indodapoer %>%</br>
filter(area_name %in% c("Banten, Prop.","DKI Jakarta, Prop.","Jawa Barat, Prop.","Jawa Tengah, Prop.","DI Yogyakarta, Prop.","Jawa timur, Prop.")) %>%
transmute(provinsi = str_remove(area_name, ", Prop."), tahun = year, pdrb_nonmigas = total_gdp_excluding_oil_and_gas_in_idr_million_constant_price) %>%
filter(!is.na(pdrb_nonmigas))</br>
glimpse(pdrb_pjawa) </summary><table><tr><td> <img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_2.jpg"></td></tr></table></details>

 <b>Grafik PDRB Non-Migas</b><details> <summary> library(dplyr)</br>
library(ggplot2)</br>
library(forcats)</br>
pdrb_pjawa %>% </br>
mutate( provinsi = fct_reorder2(provinsi, tahun, pdrb_nonmigas)) %>% </br>
ggplot(aes(tahun, pdrb_nonmigas, colour = provinsi)) + geom_line() </summary><table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (2).png"> </td></tr></table></details>


<b>Direct Labeling</b><details> <summary>>library(ggplot2)</br>
library(dplyr)</br>
library(directlabels)</br>
pdrb_pjawa %>%</br>
  ggplot(aes(tahun, pdrb_nonmigas)) +</br>
  geom_line(aes(colour = provinsi), show.legend = FALSE) +</br>
  geom_dl(</br>
    aes(label = provinsi),</br>
    method = "last.points",</br>
    position = position_nudge(x = 0.3) # agar teks tidak berhimpitan dengan garis) </summary><table><tr><td> <img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (2).png"></td></tr></table></details>

 <b>Finalisasi Grafik</b><details> <summary>> library(ggplot2)
library(dplyr)</br>
library(directlabels)</br>
library(hrbrthemes)</br>
pdrb_pjawa %>%</br>
ggplot(aes(tahun, pdrb_nonmigas / 1e6)) +</br>
geom_line(aes(colour = provinsi), show.legend = FALSE) +</br>
geom_dl(</br>
aes(label = provinsi),</br>
method = "last.points",</br>
position = position_nudge(x = 0.3) # agar teks tidak berhimpitan dengan garis
) +</br>
labs(</br>
x = NULL,</br>
y = NULL,</br>
title = "PDRB Non-Migas di Pulau Jawa Hingga Tahun 2011",</br>
subtitle = "PDRB atas dasar harga konstan, dalam satuan triliun",</br>
caption = "Data: INDO-DAPOER, The World Bank"</br> ) +
coord_cartesian(clip = "off") +</br>
theme_ipsum(grid = "Y", ticks = TRUE)</summary><table><tr><td> <img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (4).png"></td></tr></table></details>

 <b>Finalisasi Grafik</b><details> <summary>library(ggplot2)</br>
library(dplyr)</br>
library(directlabels)</br>
library(hrbrthemes)</br>
pdrb_pjawa %>%</br>
ggplot(aes(tahun, pdrb_nonmigas / 1e6)) +</br>
geom_line(aes(colour = provinsi), show.legend = FALSE) +</br>
geom_dl(</br>
aes(label = provinsi),</br>
method = "last.points",</br>
position = position_nudge(x = 0.3) # agar teks tidak berhimpitan dengan garis</br>
) +</br>
labs(</br>
x = NULL,</br>
y = NULL,</br>
title = "PDRB Non-Migas di Pulau Jawa Hingga Tahun 2011",</br>
subtitle = "PDRB atas dasar harga konstan, dalam satuan triliun",</br>
caption = "Data: INDO-DAPOER, The World Bank") +</br>
coord_cartesian(clip = "off") +</br>
theme_ipsum(grid = "Y", ticks = TRUE)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (5).png"> </td></tr></table></details>

 <p align="justify"><b>Seluas Apa</b></br>Indonesia merupakan negara kepulauan yang sangat luas, tentu Anda telah mengetahui akan hal tersebut. Namun bagaimanakah perbandingan luas provinsi-provinsi di Indonesia. Dalam data indodapoer, data luas wilayah tersedia dalam kolom total_area_in_km. Data termutakhir adalah pada tahun 2009. Dapatkah Anda mengekstrak data tersebut menjadi obyek R bernama luas_provinsi</p><details> <summary>>  </summary>
  <table><tr><td>library(dplyr)</br>
library(stringr)</br>
luas_provinsi <- indodapoer %>%</br>
filter(str_detect(area_name,"Prop")) %>%</br>
filter(year==2009) %>%</br>
transmute(</br>
provinsi=str_remove(area_name,", Prop."),</br>
luas_wilayah=total_area_in_km</br>
)</br>
glimpse(luas_provinsi) </td></tr></table></details>
  
 <b>Komparasi Luas Wilayah</b><details> <summary> </summary>
  <table><tr><td>library(treemapify)</br>
library(ggplot2)</br>
library(dplyr)</br>
luas_provinsi %>%</br>
ggplot(aes(area = luas_wilayah)) +</br>
geom_treemap() +</br>
geom_treemap_text(aes(label = provinsi)) </td></tr></table></details>

<p align="justify"> <b>Modifikasi Grafik</b></br>library(ggplot2)</br>
library(hrbrthemes)</br>
library(dplyr)</br>
library(treemapify)</br>
library(scales)</br>
luas_provinsi %>% </br>
  ggplot(aes(</br>
    area = luas_wilayah,</br> 
   fill = luas_wilayah)</br>
  ) +</br>
  geom_treemap() +</br>
  geom_treemap_text(</br>
    aes(label = provinsi), </br>
    family = "Arial Narrow",</br>
    colour = "white",</br>
    reflow = TRUE,</br>
    grow = TRUE</br>
  ) +</br>
  scale_fill_viridis_c(</br>
    guide = guide_colourbar(</br>
      barwidth = 30,</br>
      barheight = 0.8</br>
    ),</br>
    labels = label_number(</br>
      big.mark = ".", </br>
      decimal.mark = ",", </br>
      suffix = " km2")</br>
  ) +</br>
  labs(</br>
    fill = "Luas\nwilayah",</br>
    title = "Perbandingan Luas 33 Provinsi di Indonesia",</br>
    subtitle = "Berdasarkan data tahun 2009, sehingga Kalimantan Utara tidak tercantum dalam grafik",</br>
   caption = "Data: INDO-DAPOER, The World Bank"</br>
  ) +</br>
theme_ipsum() +</br>
  theme(legend.position = "bottom")  </p>
  
  <b>Perjalanan Ini</b><details> <summary>library(dplyr)</br>
library(stringr)</br>
jalan_kabkota <-</br>
indodapoer %>%</br>
filter(str_detect(area_name, ", Prop.", negate = TRUE)) %>%</br>
filter(year == 2008) %>%</br>
transmute(</br>
kabkota = area_name,</br>
jalan_rusak_parah = length_of_district_road_bad_damage_in_km_bina_marga_data,</br>
jalan_rusak_ringan = length_of_district_road_light_damage_in_km_bina_marga_data,</br>
jalan_cukup_baik = length_of_district_road_fair_in_km_bina_marga_data,</br>
jalan_sangat_baik = length_of_district_road_good_in_km_bina_marga_data)</br>
glimpse(jalan_kabkota)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_3.jpg"> </td></tr></table></details>

 <b>Pivot</b><details> <summary>library(tidyr)</br>
library(dplyr)</br>
glimpse(jalan_kabkota)</br>
jalan_kabkota <-</br>
jalan_kabkota %>%</br>
pivot_longer(</br>
cols = starts_with("jalan_"),</br>
names_to = "kondisi",/br>
names_prefix = "jalan_",</br>
values_to = "panjang_jalan")</br>
glimpse(jalan_kabkota)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_4.jpg"> </td></tr></table></details>
  
 <b>The Next Step</b><details> <summary>library(dplyr)</br>
library(stringr)</br>
jalan_kabkota <-</br>
jalan_kabkota %>%</br>
mutate(</br>
status = case_when(</br>
str_detect(kabkota, ", Kab") ~ "Kabupaten",</br>
str_detect(kabkota, ", Kota") ~ "Kota",</br>
str_detect(kabkota, "City") ~ "Kota",</br>
TRUE ~ NA_character_),</br>
kondisi = factor(</br>
kondisi,</br>
levels = c("rusak_parah", "rusak_ringan", "cukup_baik", "sangat_baik"),</br>
labels = c("Rusak parah", "Rusak ringan", "Cukup baik", "Sangat baik")))</br>
glimpse(jalan_kabkota)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_5.jpg"> </td></tr></table></details>

 <b>Grafik Kondisi Jalan</b><details> <summary>install.packages("ggridges",repos = "http://cran.us.r-project.org")</br>
library(ggplot2)</br>
library(dplyr)</br>
library(ggridges)</br>
jalan_kabkota_plot <- </br>
 jalan_kabkota %>% </br>
  ggplot(aes(panjang_jalan, kondisi)) +</br>
  facet_wrap(~status) +</br>
  geom_density_ridges_gradient(</br>
    aes(fill = after_stat(x)), </br>
    show.legend = FALSE)</br>
jalan_kabkota_plot  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (6).png"> </td></tr></table></details> 
  
<b>Transformasi Logaritmik</b><details> <summary>install.packages("ggridges",repos = "http://cran.us.r-project.org")</br>
library(ggplot2)</br>
library(dplyr)</br>
library(ggridges)</br>
jalan_kabkota_plot <-</br>
jalan_kabkota %>%</br>
ggplot(aes(panjang_jalan, kondisi)) +</br>
facet_wrap(~status) +</br>
geom_density_ridges_gradient(</br>
aes(fill = after_stat(x)),</br>
show.legend = FALSE)</br>
jalan_kabkota_plot +</br>
geom_vline(xintercept = 100, linetype = "dashed", colour = "darkslategray4") +</br>
scale_x_continuous(trans = "log10")  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (7).png"> </td></tr></table></details>

 <b>Finalisasi</b><details> <summary>install.packages("ggridges",repos = "http://cran.us.r-project.org")</br>
library(ggplot2)</br>
library(dplyr)</br>
library(ggridges)</br>
library(hrbrthemes)</br>
jalan_kabkota_plot <- </br>
  jalan_kabkota %>%</br>
  ggplot(aes(panjang_jalan, kondisi)) +</br>
  facet_wrap(~status) +</br>
  geom_density_ridges_gradient(</br>
    aes(fill = after_stat(x)),</br>
    show.legend = FALSE)</br>
jalan_kabkota_plot +</br>
  geom_vline(xintercept = 100, linetype = "dashed", colour = "darkslategray4") +</br>
  scale_x_continuous(trans = "log10") +</br>
  scale_fill_viridis_c(option = "magma") +</br>
  labs(</br>
    x = "Panjang jalan (Km)",</br>
    y = NULL,</br>
     title = "Jalan Kabupaten/Kota Berdasarkan Kondisi",</br>
subtitle = "Berdasarkan data tahun 2008, garis vertikal menunjukan panjang jalan 100 Km",</br>
caption = "Data: INDO-DAPOER, The World Bank") +</br>
  theme_ipsum(grid = FALSE, ticks = TRUE)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (8).png"> </td></tr></table></details> 
  
   <b>Fasilitas Kesehatan di Kalimantan</b><details> <summary>library(dplyr)</br>
library(ggplot2)</br>
library(tidyr)</br>
library(stringr)</br>
library(forcats)</br>
faskes_kalimantan <-</br>
indodapoer %>%</br>
filter(str_detect(area_name, "Kalimantan")) %>%</br>
filter(year == 2011) %>%</br>
transmute(</br>
provinsi = str_remove(area_name, ", Prop."),</br>
rumahsakit = number_of_hospitals,</br>
polindes = number_of_polindes_poliklinik_desa_village_polyclinic,</br>
puskesmas = number_of_puskesmas_and_its_line_services) %>%</br>
pivot_longer(</br>
cols = -provinsi,</br>
names_to = "faskes",</br>
values_to = "jumlah") %>%</br>
filter(!is.na(jumlah)) %>%</br>
mutate(</br>
provinsi = fct_reorder(provinsi, jumlah, sum),</br>
jumlah = ceiling(jumlah / 10))</br>
glimpse(faskes_kalimantan)  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/Screenshot_6.jpg"> </td></tr></table></details> 
  
<b>Waffle Charts</b><details> <summary>install.packages("waffle", repos = "https://cinc.rud.is")</br>
library(waffle)</br>
library(ggplot2)</br>
library(dplyr)</br>
faskes_kalimantan_plot <- faskes_kalimantan %>% </br>
  ggplot(aes(fill = faskes, values = jumlah)) +</br>
  facet_wrap(~provinsi) + geom_waffle(colour = "white")</br>
faskes_kalimantan_plot  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (9).png"> </td></tr></table></details>

 <b>Mengatur Warna dan Label</b><details> <summary>install.packages("waffle", repos = "https://cinc.rud.is")</br>
library(waffle)</br>
library(ggplot2)</br>
library(dplyr)</br>
faskes_kalimantan_plot <-</br>
faskes_kalimantan %>%</br>
ggplot(aes(fill = faskes, values = jumlah)) +</br>
facet_wrap(~provinsi) +</br>
geom_waffle(colour = "white")</br>
faskes_kalimantan_plot <-</br>
faskes_kalimantan_plot +</br>
scale_fill_manual(</br>
values = c(</br>
"polindes" = "seagreen3",</br>
"puskesmas" = "steelblue",</br>
"rumahsakit" = "cyan4"),</br>
labels = c(</br>
"polindes" = "Poliklinik Desa",</br>
"puskesmas" = "Puskesmas",</br>
"rumahsakit" = "Rumah Sakit")) +</br>
labs(</br>
fill = NULL,</br>
title = "Fasilitas Kesehatan di Kalimantan",</br>
subtitle = "Berdasarkan data tahun 2011, satu petak menyatakan ±10 faskes",</br>
caption = "Data: INDO-DAPOER, The World Bank")</br>
faskes_kalimantan_plot  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (10).png"> </td></tr></table></details> 


 <b>Finalisasi Waffle Charts</b><details> <summary>install.packages("waffle", repos = "https://cinc.rud.is")</br>
library(waffle)</br>
library(ggplot2)</br>
library(dplyr)</br>
faskes_kalimantan_plot <-</br>
faskes_kalimantan %>%</br>
ggplot(aes(fill = faskes, values = jumlah)) +</br>
facet_wrap(~provinsi) +</br>
geom_waffle(colour = "white")</br>
faskes_kalimantan_plot <-</br>
faskes_kalimantan_plot +</br>
scale_fill_manual(</br>
values = c(</br>
"polindes" = "seagreen3",</br>
"puskesmas" = "steelblue",</br>
"rumahsakit" = "cyan4"),</br>
labels = c(</br>
"polindes" = "Poliklinik Desa",</br>
"puskesmas" = "Puskesmas",</br>
"rumahsakit" = "Rumah Sakit")) +</br>
labs(</br>
fill = NULL,</br>
title = "Fasilitas Kesehatan di Kalimantan",</br>
subtitle = "Berdasarkan data tahun 2011, satu petak menyatakan ±10 faskes",</br>
caption = "Data: INDO-DAPOER, The World Bank")</br>
faskes_kalimantan_plot +</br>
coord_equal() +</br>
theme(</br>
text = element_text(family = "Arial Narrow"),</br>
plot.title.position = "plot",</br>
plot.title = element_text(face = "bold", size = 18, hjust = 0.5),</br>
plot.subtitle = element_text(face = "plain", size = 12, hjust = 0.5),</br>
plot.caption = element_text(face = "italic", size = 9),</br>
legend.position = "bottom",</br>
panel.background = element_blank(),</br>
panel.grid = element_blank(),</br>
strip.background = element_blank(),</br>
strip.text = element_text(face = "italic", size = 9, hjust = 0),</br>
axis.text.x = element_blank(),</br>
axis.text.y = element_blank(),</br>
axis.ticks = element_blank())  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (11).png"> </td></tr></table></details> 
  
   <b>Import Dataset</b><details> <summary>>  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (4).png"> </td></tr></table></details> 
  
   <b>Import Dataset</b><details> <summary>>  </summary>
  <table><tr><td><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download (4).png"> </td></tr></table></details> 
