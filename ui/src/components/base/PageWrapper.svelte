<script>
    export let center = false;

    let classes = "";
    export { classes as class }; // export reserved keyword
</script>

<div class="page-wrapper {classes}" class:center-content={center}>
    <main class="page-content">
        <slot />
    </main>

<!--    <footer class="page-footer">-->
<!--        <a href={import.meta.env.PB_DOCS_URL} target="_blank" rel="noopener noreferrer">-->
<!--            <i class="ri-book-open-line txt-sm" />-->
<!--            <span class="txt">Docs</span>-->
<!--        </a>-->
<!--        <span class="delimiter">|</span>-->
<!--        <a href={import.meta.env.PB_RELEASES} target="_blank" rel="noopener noreferrer" title="Releases">-->
<!--            <span class="txt">系统管理后台 {import.meta.env.PB_VERSION}</span>-->
<!--        </a>-->
<!--    </footer>-->
</div>
