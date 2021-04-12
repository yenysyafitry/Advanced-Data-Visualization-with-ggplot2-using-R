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
<tr><td><b>Paket dplyr</b></td> <td><i>Output : TRUE</i></td></tr>
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
<p align="justify"> <b></b></br>
<p align="justify"> <b>Mengingat Kembali</b></br>
<p align="justify"> <b></b></br>
<p align="justify"> <b>Mengingat Kembali</b></br>
<p align="justify"> <b></b></br>
<p align="justify"> <b>Mengingat Kembali</b></br>
<p align="justify"> <b></b></br>
