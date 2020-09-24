$(function() {
    $('a[href^="http"]').not('a[href*=smwenku\\.com]').attr('target','_blank');
    $('.article-content pre.cpp').removeClass('cpp').addClass('language-cpp');
    $('.article-content .dp-cpp').parent("pre").addClass('language-cpp');
    //Prism.highlightAll();
    $('.article-content img[src*="51cto.com"]').each(function(i, e) {
	$(e).attr('referrerpolicy', 'no-referrer');
	$(e).attr("src", $(e).attr("src"));
    })
});


