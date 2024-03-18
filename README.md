Page ViewCount Function JavaScript Call in HTMl Script
====================================
▚ A beautifully simple to set up Function that gives site visitors and site owners the ability to quickly and easily see how many people have visited that page or post. This function records the date of visits to the "moment-jalaali"

▚  Just put the script function in the format HTML so that a record is saved in your MongoDB with each user visit.


## ▚ simple Function Call in HTMl Script :

```html
<!DOCTYPE HTML>
<html lang="fa">

<body>
    <script>
        (function () {
            axios('https://***/updateViewCount/WebSiteName').then(resp => {})
        })();
        (function () {
            axios('https://***/updateViewCount/PageName').then(resp => {})
        })();
    </script>
</body>

</html>
```

#
Please Leave Your Comments if You Got Some Trouble.

