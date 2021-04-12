<p align="justify"> Bahasa R memiliki berbagai sistem untuk membuat grafik, baik grafik statis maupun grafik interaktif. Pada grafik statis misalnya terdapat sistem base, lattice, ggplot, dan lainnya. Di dalam modul ini Anda akan diajak untuk mengenal salah satu sistem grafik yang paling populer di R, yaitu ggplot.
Sistem pembuatan grafik dengan ggplot dapat dilakukan dengan menggunakan paket ggplot2 yang merupakan implementasi dari konsep Grammar of Graphic untuk bahasa pemrograman R. Pada prinsipnya, konsep Grammar of Graphic mengajak kita untuk merekonstruksi pembuatan grafik dengan menggunakan kaidah tata bahasa sehingga tidak terikat pada nama jenis grafik (contoh: scatterplot, line-chart, bar-chart, dll.) </p>
<p align="justify"> <b>Konsep Grammar of Graphic</b></br>library(ggplot2)</p>
<p align="justify"> <b>Mengingat Kembali</b></br>library(ggplot2)</br>
qplot(x = carat, y = price, colour = clarity, data = diamonds)</br><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download.png"></p>
<p align="justify"> <b>Pembuatan Grafik dengan ggplot</b></br>library(ggplot2) </br> ggplot(data = diamonds, mapping = aes(x = carat, y = price, colour = clarity)) +
  geom_point()</br><img src="https://github.com/yenysyafitry/Advanced-Data-Visualization-with-ggplot2-using-R/blob/main/download(1).png"></p>
<p align="justify"> <b>Kode 3 Rupa</b></br>library(ggplot2)diamonds_c1 <- ggplot( data = diamonds, mapping = aes(x = carat, y = price, colour = clarity)) + geom_point()
summary(diamonds_c1)</br>
diamonds_c2 <- ggplot(data = diamonds) + geom_point(mapping = aes(x = carat, y = price, colour = clarity))</br>
summary(diamonds_c2)</br>
diamonds_c3 <- ggplot() + geom_point( data = diamonds,mapping = aes(x = carat, y = price, colour = clarity))</br>
summary(diamonds_c3)</p>
<p align="justify"> <b>Yin dan Yang</b></br>library(dplyr)	</p>
<p align="justify"> <b>Paket dplyr</b></p>Terdapat beberapa fungsi utama dari paket dplyr untuk melakukan transformasi data, diantaranya:
<ol>
<li>select()</li>
<li>filter()</li>
<li>arrange()</li>
<li>mutate()</li>
<li>summarise()</li>
<li>group_by()	</li></ol></p>
<details>
  <summary><b>Paket dplyr</br></b>
</summary>
<table border="0">TRUE</table>
</details>
<p align="justify"> <b>Mengingat Kembali</b></br>
<p align="justify"> <b></b></br>
