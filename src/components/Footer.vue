<template>
    <footer id="footer">
        <ul id="contact-info">
            <li>Developed by: Zoey Kerr</li>
            <li>contact: zkerr4032@outlook.com</li> 
            <li>github: <a href="https://www.github.com/Mandatorytoast">Mandatorytoast</a></li>
        </ul>
    </footer>
</template>

<style>
#footer{
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    height: 150px;
    width: 100%;
    position: fixed;
    display: flex;
    flex-direction: row;
    color: white;
    justify-content: flex-end;
    align-items: flex-end;
}
#contact-info{
    list-style: none;
    padding-right:40px;
}
#contact-info a {
    color: #65ccb8;
}
#contact-info a:visited {
    color: #65ccb8;
}
</style>
