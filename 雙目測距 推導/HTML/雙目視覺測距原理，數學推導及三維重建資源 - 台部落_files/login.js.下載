$(document).ready(function(){
    // initialize login form
    var form = $('#login-form');
    var login_base_action = form.attr('action');
    var login_action = login_base_action + '?src=' + encodeURI(window.location.href);
    form.attr('action',login_action);

    // handle login layer toggling
    $('.login, #login-layer-wrapper.overlay, .login-layer-close-button').click(function(e){
        e.preventDefault()
        toggleLoginLayer();
    });

    $('.login-layer').click(function(e){
        e.stopPropagation();
    });

    form.submit(function(e) {
        e.preventDefault();

        var email = $('#login-form input[name=email]').val();
        var passwd = $('#login-form input[name=passwd]').val();
        if (email === '' || passwd === '') {
            $('.err-info').css('display', 'block')
        } else {
            form[0].submit();
        }
    });

if ( getUrlParam('login') == 1 ) {
    toggleLoginLayer();
    }
});

function toggleLoginLayer() {
    var overlay = $('#login-layer-wrapper.overlay')
    if(overlay.hasClass('active')){
        overlay.removeClass('active');
    } else {
        overlay.addClass('active');
    }
}


$.each($("a#logout,a#m-logout"), function(index,value) {
    var _href = $(this).attr("href");
    $(this).attr("href", _href + '?next=' + document.URL);
});