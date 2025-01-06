// ==UserScript==
// @name         Stop Audio Autoplay with Debugging
// @namespace    http://tampermonkey.net/v
// @version      1.5
// @description  Stops audio autoplay, resets to the start, and ensures manual playback works.
// @author       Khubaib Jan
// @match        *://*.humanatic.com/*
// @grant        none
// @run-at       document-end
// ==/UserScript==

(function() {
    'use strict';

    // Debugging function to log audio status
    function logAudioStatus(audio, message) {
        console.log(`${message} - Paused: ${audio.paused}, Current Time: ${audio.currentTime}, Autoplay: ${audio.autoplay}`);
    }

    // Function to stop autoplay and reset the audio
    function stopAutoplay() {
        const audios = document.querySelectorAll('audio');
        audios.forEach(audio => {
            logAudioStatus(audio, "Checking audio element");

            // Check if the audio is autoplaying
            if (!audio.paused && audio.currentTime > 0) {
                audio.pause();           // Pause the audio
                audio.currentTime = 0;   // Reset playback to the beginning
                logAudioStatus(audio, "Autoplay stopped and reset to start");
            }

            // Ensure autoplay is disabled
            if (audio.autoplay) {
                audio.removeAttribute('autoplay');
                console.log("Autoplay attribute removed.");
            }
        });
    }

    // Monitor for dynamically added audio elements
    const observer = new MutationObserver((mutations) => {
        mutations.forEach(mutation => {
            mutation.addedNodes.forEach(node => {
                if (node.tagName === 'AUDIO') {
                    stopAutoplay();
                }
            });
        });
    });

    observer.observe(document.body, {
        childList: true,
        subtree: true
    });

    // Stop autoplay for already loaded audios after DOM is fully loaded
    window.addEventListener('DOMContentLoaded', () => {
        stopAutoplay();
    });

    // Log any clicks on audio elements for debugging
    document.body.addEventListener('click', (event) => {
        if (event.target.tagName === 'AUDIO') {
            console.log("Audio element clicked:", event.target);
        }
    });
})();
