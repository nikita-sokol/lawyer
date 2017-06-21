$(function(){
    $(".box_skitter_large").skitter({ // http://thiagosf.net/projects/jquery/skitter/
                        animation: 'random',
                        interval: 3000,
 			dots: true, 
                        navigation: false,
                        numbers_align: "center", 
                        numbers: false,
 			progressbar: false, 
                        label: false,
                        animateNumberActive: { backgroundColor:'#fff', color:'#E7B45B' },
                        animateNumberOut:    { backgroundColor:'#A3A3A3', color:'#A3A3A3' },
                        animateNumberOver:   { backgroundColor:'#fff', color:'#E7B45B' }
		});

    var imgCount = 0;
    $('#article.page img').each(function(){
        if (!$(this).parent('a').length || ($(this).parent('a').length && $(this).parent('a').attr('href') == $(this).attr('src'))) {
            imgCount++;
        }
    });
    $('#article.page img').each(function(){
		var class_img = $(this).attr('style');
        var fb = (!$(this).parent('a').length || ($(this).parent('a').length && $(this).parent('a').attr('href') == $(this).attr('src')));
        $(this).attr('style', '').wrap('<a href="'+(!fb?$(this).parents('a').attr('href'):$(this).attr('src'))+'"'+(imgCount>1?' rel="fg"':'')+' class="fancyb" style="'+class_img+'">');
        var title = $(this).attr('title');
        var desc = $(this).attr('longdesc');
		
		if(title){
			$(this).parent().append('<span class="product-desc"'+(desc ? 'title="'+desc+'"' : '')+'>'+title+'</a>');
		}
		
    });
    $('a.fancyb', $('#article')).fancybox({ // http://fancyapps.com/fancybox/
        helpers : {
            title : {
                type : 'inside'
            }
        },
		beforeShow: function() {
            this.title = function(){
				var img_fb = $(this).find('img');
				return (img_fb.attr('title') ? '<strong>'+img_fb.attr('title')+'</strong>' : '')+(img_fb.attr('longdesc') ? img_fb.attr('longdesc') : '');
			}
        }
    });
	
		
	var CALL_ME_FORM = {
		init: function(){
			CALL_ME_FORM.toggle();
			CALL_ME_FORM.hideClickDocument();
		},
		toggle: function(){
			var call_me_block = $('#call-me');
			
			call_me_block.find('.call-me-ico').click(
				function(){
					if(call_me_block.hasClass('close')){
						call_me_block
							.stop(true)
							.animate({right: '0'})
							.removeClass('close')
							.addClass('open');
						return false;
					}else{
						call_me_block
							.stop(true)
							.animate({right: '-'+CALL_ME_FORM.blockWidth+'px'})
							.removeClass('open')
							.addClass('close');
						return false;
					}
				}
			);
		},
		hideClickDocument: function(){

			var call_me_block = $('#call-me');
								
			call_me_block.click(
				function(e){
					e.stopPropagation();
				}
			);
				
			$(document).click(
				function(){
					call_me_block
						.stop(true)
						.animate({right: '-'+CALL_ME_FORM.blockWidth+'px'})
						.removeClass('open')
						.addClass('close');
				}
			);
		},
		blockWidth: 272
	};
	
	
	CALL_ME_FORM.init();

	
    
    // feedback
    $('a[href="#form-feedback"]').click(function(){
        if ($('#form-feedback').css('display') == 'block') {
            $('#form-feedback').fadeOut('fast');
        } else {
            $('#form-feedback').css({
                top: (parseInt($(this).offset().top + ($(this).height() / 2)) - 282) + 'px'
            }).fadeIn('fast');
        }
        return false;
    });
	
    $("#form_contact").submit(function(e){
        var hasError = false
		var ajax = true;
		
		var this_form = $(this);
		
        this_form.find('input.required, textarea.required').each(function(){
            $(this).removeClass('error');
            if ($(this).val() == '') {
                $(this).addClass('error');
                hasError = true;
            }
        });
        if (hasError) {
            msg.show('Заполните все поля формы!');
            return false;
        } else if (ajax) {
            e.preventDefault();
            $.post(this_form.attr('action'), this_form.serialize(), function(data){
                if(data.success) {
                   msg.show('Спасибо, сообщение успешно отправлено!');
				   this_form.find('input[type=text], textarea').val('');
                } else {
                    if(!data.captcha){
                        $('#captcha-form').addClass('error');
                    }
                }
            }, 'json');
            return false;
        }
    });
        
    msg.init();
    
    $('.qnt > .qnt_input').click(function(){ this.select() });
    $('.qnt > .plus').click(function(){
		var qnt_input = $(this).parent().find('.qnt_input');
        var val = parseInt(qnt_input.val())+1;  
        qnt_input.val(val);
    });
    $('.qnt > .minus').click(function(){
		var qnt_input = $(this).parent().find('.qnt_input');
        var val = parseInt(qnt_input.val())-1;
        qnt_input.val(val > 0 ? val : 1);
    });
    
    $('.product .img > a, .product h2 > a').click(function(){
        var link = $(this).attr('href');
        $('#modalwrap').fadeIn('fast');
        $('#modal-card').css('top', ($(window).scrollTop() + 50) + 'px');
        $('<div>').load(link + ' .product-view', function(){
            $('#modal-card').html($(this).html());
            $('#modal-card').fadeIn('fast');
            $('#modal-card a.pfb').fancybox({helpers:{title:{type:'inside'}}});
        });
        return false;
    });
    
    $('.cart-block .qnt > .plus, .cart-block .qnt > .minus').click(changeCount);
    $('.cart-block .qnt > .qnt_input').change(changeCount);
    
    $('.cart-block .delete').click(function(){
        var id = parseInt($(this).parents('div.cart-block').attr('id').replace('row',''));
        $.ajax({
            type: 'POST',
            url: '/site/cartDelete',
            data: {id: id},
            success: function(result){
                $('#row'+id).remove();
                recalcCart(jQuery.parseJSON(result))
            }
         });
    });
    
    $('.confirm').click(function(){
        var form = $('.cart-view > form');
        form.find('.error').removeClass('error');
        if (form.find('[name=name]').val() == '') {
            form.find('[name=name]').prev().addClass('error');
            form.find('[name=name]').focus();
            return false;
        }
        if (form.find('[name=phone]').val() == '') {
            form.find('[name=phone]').prev().addClass('error');
            form.find('[name=phone]').focus();
            return false;
        }
        $.post('/site/zakaz', form.serialize(), function(data){
            msg.show('Спасибо, Ваш заказ принят!');
            setTimeout(function(){ location.href='/'; }, 1000);
        });
    });
    
    $('.tocart').click(function(){
        var id = parseInt($(this).attr('id').replace('tocart',''));
        var amount = 1;
        if ($("#row"+id).find('.qnt > .qnt_input').length) {
            amount = parseInt($("#row"+id).find('.qnt > .qnt_input').val());
            amount = amount > 1 ? amount : 1;
            $("#row"+id).find('.qnt > .qnt_input').val(amount);
        }
        $.ajax({
            type: 'POST',
            url: '/site/cartAdd',
            data: {id: id, amount: amount},
            success: function(result){
                resp = jQuery.parseJSON(result);
                recalcCart(resp);
                msg.show(resp['result']);
            }
        });
    });
		
});


function changeCount() {
	var cart_block = $(this).closest('.cart-block');
    var id = parseInt(cart_block.attr('id').replace('row',''));
    var count = parseInt(cart_block.find('.qnt_input').val());
    if (count) {
        $.post('/site/cartChangeCount', {id: id, col: count}, function(data){
            recalcCart(data, id);
        }, 'json');
    }
}

function clearcart() {
    $.cookie('cart', null, {path: '/', expires: 31});
    $.cookie('cost', 0, {path: '/', expires: 31});
    recalcCart({cost: 0, count: 0});
}

function recalcCart(data, id) {
//    if ($('.cart-view').length) {
        if (id && data[id]) $('#row' + id + ' .product-total').text(data[id] + ' р.');
        $('.order-total-cart').text('Итого: '+data['cost']+' р.');
//    }
    if (typeof data['count'] != 'undefined') {
        $('#countincart').text(plural_str(data['count'],'{n} товар','{n} товара','{n} товаров'));
    }
    $('#totalallcart').text(data['cost']+' р.');
}

var msg = {
    html: '<div id="modal-wrap"></div><div id="modal-msg"><a href="#" class="close" title="Закрыть">&times;</a><div class="content"></div></div>',
    show: function(text, closeTimeout) {
        $('#modal-msg').css('top', ($(window).scrollTop() + 200) + 'px');
        if (typeof closeTimeout == "undefined") {
            closeTimeout = 1000;
        }
        $('#modal-wrap').fadeIn('fast', function(){
            $('#modal-msg').html(text);
            $('#modal-msg').fadeIn('fast', function(){ 
                if (closeTimeout !== false) {
                    setTimeout(function(){msg.close()}, closeTimeout);
                }
            });
        });
    },
    close: function() {
        $('#modal-msg').fadeOut('fast', function(){
            $('#modal-wrap').fadeOut('fast');
        });
    },
    init: function() {
        if ($('#modal-msg, #modal-wrap').length == 0) {
            $('body').append(this.html);
        }
        $('#modal-wrap').click(function(){ msg.close() });
    }
};

function plural_str(i, str1, str2, str3) {
    function plural (a){
        if ( a % 10 == 1 && a % 100 != 11 ) return 0
        else if ( a % 10 >= 2 && a % 10 <= 4 && ( a % 100 < 10 || a % 100 >= 20)) return 1
        else return 2;
    }
    switch (plural(i)) {
        case 0:return str1.replace('{n}',i);
        case 1:return str2.replace('{n}',i);
        default:return str3.replace('{n}',i);
    }
}
 // Cart dropdown
   $(function(){
	//menu_slider();
});

/*function menu_slider(parent_menu){
	parent_menu = parent_menu ? parent_menu : '#sidebar';
	$('.menu ul', $(parent_menu)).hide();
	var active_li = $('.menu li.active', $(parent_menu));
	if(active_li.length > 0){
		active_li.parents('ul').show();
		active_li.find('> ul').show();
	}
	$('.menu li a', $(parent_menu)).click(
		function(){
			var parent_li = $(this).parents('li:first');
			var sub_menu = parent_li.find('> ul');
			if(sub_menu.length < 1) return;			
			if(sub_menu.hasClass('open')) return;			
			sub_menu.find('ul').hide();		
			sub_menu.slideDown(600,
				function(){
					sub_menu.addClass('open');
				}
			);			
			return false;
		}
	);
}*/

$('document').ready(
	function() {
		$('.call-back-form-link').fancybox({type: 'ajax',padding:0,beforeShow:function(){$('.fancybox-skin').removeClass('fancybox-image').addClass('fancybox-form');}/*width:'460',autoSize:false*/});
	}
);

$(document).ready(function(){
            
            //Check to see if the window is top if not then display button
            $(window).scroll(function(){
                if ($(this).scrollTop() > 100) {
                    $('.scrollToTop').fadeIn();
                } else {
                    $('.scrollToTop').fadeOut();
                }
            });
            
            //Click event to scroll to top
            $('.scrollToTop').click(function(){
                $('html, body').animate({scrollTop : 0},800);
                return false;
            });
            
        });