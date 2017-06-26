(function($){

	$(document).ready(function(){
		function replaceText(oldText, newText, node){ 
		  node = node || document.body; 
		
		  var childs = node.childNodes, i = 0;
		
		  while(node = childs[i]){ 
			if (node.nodeType == Node.TEXT_NODE){ 
			  node.textContent = node.textContent.replace(oldText, newText); 
			} else {
			  var $node = $(node);
			  if ($node.attr('id') !== 'wiki_page_show' && !$node.hasClass('context_module'))
				replaceText(oldText, newText, node); 
			  if (node.nodeName === "INPUT") {
			  	  var p = node.getAttribute("placeholder");
			  	  if (typeof p === 'string') {
			  	  	node.setAttribute("placeholder", p.replace(oldText, newText));
			  	  }
			  }			  
			} 
			i++; 
		  } 
		}
		
		function doReplace(node) {
			replaceText('Take the Quiz', 'Take Reflection', node);
			replaceText('Take the Survey', 'Take Reflection', node);
			replaceText('Quizzes', 'Reflection', node);
			replaceText('Modules', 'Content', node);
			replaceText('Quiz', 'Reflection', node);
			replaceText('Module', 'Content', node);
			replaceText('Surveys', 'Reflection', node);
			replaceText('Survey', 'Reflection', node);
			
			
			replaceText('quizzes', 'reflection', node);
			replaceText('modules', 'content', node);
			replaceText('quiz', 'reflection', node);
			replaceText('surveys', 'reflection', node);
			replaceText('survey', 'reflection', node);
			replaceText('module', 'content', node);
		}

		doReplace();
		
		var oldOpen = $.ui.dialog.prototype.open;
		$.ui.dialog.prototype.open = function() {
			doReplace(this.uiDialog[0]);
			var retval = oldOpen.apply(this, arguments);
		};
		
		$("#school-branch-images").each(function(){
			$("#main").css({"background-color":"#BEC0C2"});
		});
		
		$('#principal-prework-homepage,#leadership-prework-homepage').each(function(){
			localStorage.setItem('prework-homepage-link', $(this).attr('id'));
		});
			
		$('#homepage-link').each(function(){
			var homepageLink = localStorage.getItem('prework-homepage-link');
			if (homepageLink) {
				$(this).attr('href', '../wiki/' + homepageLink);
			}
		});
		
		$('#nlremoveframe iframe').load(function(){
			$('.user_content', this.contentDocument).appendTo('#nlframecontent');
		});
		
		if (window.ENV && ENV.current_user_roles) {
		     var isStudent = false;
		     $.each(ENV.current_user_roles, function(i, role) {
		     	 if (role === 'student') {
		     	 	  isStudent = true;
		     	 }
		     });
		     if (isStudent) {
		     	$('body').addClass('nl-student');
		     } else {
		        $('body').addClass('nl-non-student');
		    }
		}
	});
	
	require(["https://cdn.rawgit.com/BrockA/2625891/raw/9c97aa67ff9c5d56be34a55ad6c18a314e5eb548/waitForKeyElements.js"], function() {
		var attachRowSorter = function() {
		  $('.rubric_table tbody').sortable({
			axis: 'y',
			containment: 'parent',
			items: '> tr'
		  });
		};
		if (window.location.href.match(/rubrics/)) {
			waitForKeyElements('.rubric_container.rubric.editing', attachRowSorter);
		}
	});

})(jQuery);

// ==UserScript==
// @name        Sort a Rubric
// @namespace   https://github.com/jamesjonesmath/canvancement
// @description This program allows the user to sort rubric criteria
// @include     https://*.instructure.com/courses/*/rubrics/*
// @require     https://gist.github.com/raw/2625891/waitForKeyElements.js
// @version     2
// @grant       none
// ==/UserScript==

(jQuery);

(function() {var walkme = document.createElement('script'); walkme.type = 'text/javascript'; walkme.async = true; walkme.src = 'https://cdn.walkme.com/users/815ec55b68e94750b20b4cdc1b2f6f6a/walkme_815ec55b68e94750b20b4cdc1b2f6f6a_https.js'; var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(walkme, s); window._walkmeConfig = {smartLoad:true}; })();