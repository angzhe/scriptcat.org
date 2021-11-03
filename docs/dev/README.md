// ==UserScript==
// @name         超星解除倍速限制
// @namespace    https://bbs.tampermonkey.net.cn/
// @version      0.1.0
// @description  try to take over the world!
// @author       You
// @match        https://mooc1-2.chaoxing.com/ananas/modules/video/index.html*
// @run-at       document-start
// @grant        none
// ==/UserScript==

window.oldace = null;
window.videoobj = []

document.addEventListener('readystatechange', () => {
    if (window.videojs !== undefined && window.oldace === null) {
        console.log('检测到了', window.videojs)
        window.oldace = window.videojs
        window.videojs = function (...args) {
            let ret = window.oldace.call(this, ...args)
            window.videoobj.push(ret)
            ret.laston = ret.on;
            ret.on = function (...args) {
                console.log('调用了on', args);
                if (args[0] == 'ratechange') {
                    console.log('屏蔽限制');
                    return;
                }
                return this.laston(...args)
            }
            console.log('videojs', ret)
            return ret
        }
    }
});
