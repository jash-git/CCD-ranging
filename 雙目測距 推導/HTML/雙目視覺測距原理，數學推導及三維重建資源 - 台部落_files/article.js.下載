$(window).on("load",function () {
        var comment_id = getUrlParam('commentScrool');
        if (comment_id) {
            commentScrool_selector = '[data-comment-id="' + comment_id + '"]';
            $([document.documentElement, document.body]).animate({
                scrollTop: $(commentScrool_selector).offset().top
            }, 300);
        }
    }
)

$(document).ready(function(){
        if (is_login()) {
            article_vote();
        } else {
            $(".vote").click(function(){
                toggleLoginLayer();
            });
        }
    }
)

function article_vote() {
    $(".vote").click(function(){
        let action;
        let article_vote_button = $(this).find("i");
        let article_vote_other_button = $(this).siblings(".vote").find("i");
        let vote_up_number;
        if(article_vote_button.parent().hasClass("vote-up")) {
            vote_up_number = article_vote_button.attr('vote_up_number');
        } else {
            vote_up_number = article_vote_other_button.attr('vote_up_number');
        }

        if (typeof vote_up_number !== typeof undefined && vote_up_number !== false) {
            vote_up_number = parseInt(vote_up_number);
        } else {
            vote_up_number = 0;
        }
        if (article_vote_other_button.hasClass("active")){
            article_vote_other_button.removeClass("active");
        }
        if(!article_vote_button.hasClass("active")){
            article_vote_button.addClass("active");
            if(article_vote_button.parent().hasClass("vote-up")){
                vote_up_number = vote_up_number + 1;

                if (vote_up_number > 0 ) {
                    article_vote_button.attr('vote_up_number', vote_up_number)
                } else {
                    article_vote_button.removeAttr('vote_up_number')
                }
                action = 'article_like';
            } else {
                console.log(vote_up_number)
                vote_up_number = vote_up_number - 1;
                console.log(vote_up_number)
                if (vote_up_number > 0 ) {
                    article_vote_other_button.attr('vote_up_number', vote_up_number)
                } else {
                    article_vote_other_button.removeAttr('vote_up_number')
                }
                action = 'article_unlike';
            }
        } else {
            article_vote_button.removeClass("active");
            if(article_vote_button.parent().hasClass("vote-up")) {
                vote_up_number = vote_up_number - 1;
                if (vote_up_number > 0 ) {
                    article_vote_button.attr('vote_up_number', vote_up_number)
                } else {
                    article_vote_button.removeAttr('vote_up_number')
                }
                action = "undo_article_like";
            } else{
                action = "undo_article_unlike";
            }
        }

        $.ajax({
            url: '/api/article',
            headers: {'X-XSRFToken' : token },
            data: {post_id: post_id ,user_id:user_id,action:action},
            type: "POST",
        });
    });
}