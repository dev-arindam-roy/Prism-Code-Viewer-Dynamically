# PRISM CODE VIEWER

## Application prism code viewer with dynamic rendering

### Check Application
[https://dev-arindam-roy.github.io/Raw-Json-File-Render/](https://dev-arindam-roy.github.io/Raw-Json-File-Render/)


### Tricks is Here!!
```js
/*retrive copied clipboard data. its return a promise*/
async function retriveClipboardCopiedData() {
    return await navigator.clipboard.readText();
}

$('body').on('click', '.dynamic-render-btn', async function () {
    let _this = $(this);
    _this.attr('disabled', 'disabled');

    /*prism copy toolbar buttion force triggered*/
    let _copyBtn = _this.parents('.row').find('.code-block').find('.copy-to-clipboard-button');
    _copyBtn.trigger('click');

    /*get the code tag class name*/
    let _codeClass = _this.parents('.row').find('.code-block').find('code').attr('class');
    
    /*display loading placeholder*/
    _this.parents('.row').find('.code-block').html(displayPlaceholder());
    
    /*get the copied content*/
    let _paste = ''; 
    await retriveClipboardCopiedData().then((data) => {
        _paste = data;
    }).catch((error) => {
        console.log(error);
    });

    setTimeout(() => {
        if (_codeClass == 'language-markup') {
            _this.parents('.row').find('.code-block').html(`<pre class="line-numbers" style="border-radius: 0; margin-top: 0;"><code class="${_codeClass}"><xmp>${_paste}</xmp></code></pre>`);
        } else {
            _this.parents('.row').find('.code-block').html(`<pre class="line-numbers" style="border-radius: 0; margin-top: 0;"><code class="${_codeClass}">${_paste}</code></pre>`);
        }
        Prism.highlightAll();
        _this.removeAttr('disabled');
    }, 4000);
});
```