// hidemenu breakpoint: 960
$(window).on('load', function(){
    $('ins.adsbygoogle').css("overflow","hidden");
    }
);

$(document).ready(function() {
    $('#backTop').click(backTop);
    if (is_scroll) {
        $('#pagination').hide();
    }

    if ($(window).width()>720) {
        sticky_sidebar();
    } else {
            if (is_scroll) {
                $('.secondary-content').hide();
            }
    };
    // sticky menu
    setMenuOffset();

    $(window).scroll(function() {
        if($(window).width() > 960) {
            toggleStickyMenu(window.menuOffsetTop, window.menuOffsetLeft);
        }
         showbackTop();
    });

    $(window).resize(function(){
        refreshMenuOffsetTop();
    });
    // sticky menu end

    // toggle mobile menu
    $("#menu-icon").click(function() {
        toggleMobileMenu();
    })

    $(".mobile-menu-background").click(function(){
        toggleMobileMenu();
    })

    $(".mobile-menu-close").click(function(){
        toggleMobileMenu();
    })
    // toggle mobile menu end

    // toggle toggle google search
    $(".search-toggle").click(function(){
        toggleGoogleSearch();
    })
    // toggle toggle google search end
})

function setMenuOffset() {
    window.menuOffsetTop = $('.content').offset().top;
    window.menuOffsetLeft = $('.content').offset().left;
}

// stickey menu main function
function toggleStickyMenu() {
    var scrollTop = $(window).scrollTop();
    if (scrollTop >= window.menuOffsetTop) {
        stickMenu()
    } else {
        unstickMenu();
    }
}

function stickMenu() {
    var menuWidth= $('#menu-wrapper').width();
    if(!$('#menu-wrapper').hasClass('sticky')) {
        $('#menu-wrapper').addClass('sticky');
        $('#menu-wrapper').css("left", window.menuOffsetLeft + 'px') ;
        $('.main-content').css("margin-left", menuWidth + 'px');
    } else {
        $('#menu-wrapper').css("left", window.menuOffsetLeft + 'px') ;
    }
}

function unstickMenu() {
    if($('#menu-wrapper').hasClass('sticky')) {
        $('#menu-wrapper').removeClass('sticky');
        $('.main-content').css("margin-left", 0 + 'px') ;
    }
}

// sticky menu: 屏幕resize的时候更新window.menuOffsetTop， 因为他的offsetTop可能根据屏幕尺寸变化而变化
function refreshMenuOffsetTop() {
    setMenuOffset();
    if ($(window).width() > 960) {
        if ($('#menu-wrapper').hasClass('sticky')){
            stickMenu();
        } else {
            unstickMenu();
        }
    } else {
        if ($('#menu-wrapper').hasClass('sticky')){
            unstickMenu();
        }
    }
}

function toggleMobileMenu() {
    if($('.mobile-menu-background').hasClass("active")) {
        $('.mobile-menu-background').removeClass("active");
        $('.mobile-menu').removeClass("active");
        $('body').removeClass("noscroll");
    } else {
        $('.mobile-menu-background').addClass("active");
        $('.mobile-menu').addClass("active");
        $('body').addClass("noscroll");
    }
}

function toggleGoogleSearch(){
    if ($('#google-search-layer').hasClass('active')) {
        $('#google-search-layer').removeClass('active')
        $('.search-toggle img').attr('src','/static/svgs/search-grey-bold.svg')
    } else {
        $('#google-search-layer').addClass('active')
        $('.search-toggle img').attr('src','/static/svgs/cross.svg')
    }
}


function sticky_sidebar() {
        let el = $('.secondary-content')
        let el_height = 0;
        el.children().each(function(){el_height = el_height + $(this).outerHeight(true);});
        var el_width = el.width();
        var window_height = $(window).height();
        var el_right = el.offset().left;
        var el_offset_top = el.offset().top;
        let is_sticky = false;
        if (el_height < window_height) {
            var fixed_css = {"position": "fixed", "top": 0, "left": el_right}
        } else {
            var fixed_css = {"position": "fixed", "bottom": 0, "left": el_right}
        }
        function sticky() {
              if (!is_sticky) {
                        let c = el.clone();
                        c.html('');
                        el.after(c);
                        is_sticky = c;

                    }
                    el.css(fixed_css);
        }
        function unsticky(){
                   if (is_sticky) {
                        el.removeAttr('style');
                        is_sticky.remove();
                        is_sticky = false;
                    }
        }
            $(window).scroll(function () {
                let scrollTop = $(window).scrollTop();
                if (el_height < window_height) {
                    sticky_condition = scrollTop > el_offset_top;
                } else {
                    sticky_condition = scrollTop  + window_height > el_offset_top + el_height;
                }
                if (sticky_condition) {
                    sticky();
                } else {
                    unsticky();
                }
            })

        $(window).resize(function () {

            }
        )
};

function showbackTop() {
    let window_height = $(window).height();
    let scrollTop = $(window).scrollTop();
    //console.log(window_height,scrollTop)
    if (scrollTop > window_height ) {
        //$('#backTop').css({"position": "fixed"});
        $('#backTop').show();

    } else {
        //$('#backTop').css({"position": "static"});
        $('#backTop').hide();
    }
}
function backTop () {
    scrollTo(0,0);
}


$(document).ready(function() {
    let url = location.href;
    $("ul.menu li a").each(function () {
                let element_url = $(this).prop('href');
                if  (element_url == url) {
                    $("ul.menu li a").removeClass('menu-active');
                    $(this).addClass('menu-active');
                }
        });

});
