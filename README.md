## FOUT example for create-react-app

UPDATE: solution below

This is example repository for the [Issue #2987](https://github.com/facebookincubator/create-react-app/issues/2987).

If your add custom font to React app (like Dan Abramov expained [on stackoverflow](https://stackoverflow.com/questions/41676054/how-to-add-fonts-to-create-react-app-based-projects)), you will get following behavior during first page load.

![FOUT example](/fout.gif "FOUT example")

As a user, I expect page load without dragging (FOUT, FOIT etc.)

This is happening, because fonts are loaded asyncronusly. Synchronous font loading must solve the problem. For example, if you use google fonts, you can just do the following:

```css
@import url('https://fonts.googleapis.com/css?family=Open+Sans');
```

The most reasonable solution is to use [fontfaceobserver](https://github.com/bramstein/fontfaceobserver) and defer rendering before fonts are loaded:

```css
@font-face {
  font-family: 'Myfont';
  font-style: normal;
  font-weight: 400;
  src: url('./Myfont.ttf');
}
```

```javascript
import './index.css';
import FontFaceObserver from 'fontfaceobserver';

new FontFaceObserver('Myfont').load().then(() => {
  ReactDOM.render(<App />, document.getElementById('root'));
});
```
