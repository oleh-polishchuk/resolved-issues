# resolved-issues
The list of issues that I successfully resolved

#### :question: Disable window scrolling while swiping horizontally [MOBILE]

:white_check_mark: For React Class component:

```
componentDidMount(){
    window.addEventListener('touchstart', this.touchStart);
    window.addEventListener('touchmove', this.preventTouch, {passive: false});
}

componentWillUnmount(){
    window.removeEventListener('touchstart', this.touchStart);
    window.removeEventListener('touchmove', this.preventTouch, {passive: false});
}

touchStart(e){
    this.firstClientX = e.touches[0].clientX;
    this.firstClientY = e.touches[0].clientY;
}

preventTouch(e){
    const minValue = 5; // threshold

    this.clientX = e.touches[0].clientX - this.firstClientX;
    this.clientY = e.touches[0].clientY - this.firstClientY;

    // Vertical scrolling does not work when you start swiping horizontally.
    if(Math.abs(this.clientX) > minValue){ 
        e.preventDefault();
        e.returnValue = false;
        return false;
    }
}
```

:white_check_mark: For React Functional component:
```
  const [firstClientX, setFirstClientX] = useState();
  const [firstClientY, setFirstClientY] = useState();
  const [clientX, setClientX] = useState();

  useEffect(() => {
    const touchStart = e => {
      setFirstClientX(e.touches[0].clientX);
      setFirstClientY(e.touches[0].clientY);
    };

    const preventTouch = e => {
      const minValue = 5; // threshold

      setClientX(e.touches[0].clientX - firstClientX);

      // Vertical scrolling does not work when you start swiping horizontally.
      if (Math.abs(clientX) > minValue) {
        e.preventDefault();
        e.returnValue = false;
        return false;
      }
    };

    window.addEventListener('touchstart', touchStart);
    window.addEventListener('touchmove', preventTouch, { passive: false });
    return () => {
      window.removeEventListener('touchstart', touchStart);
      window.removeEventListener('touchmove', preventTouch, {
        passive: false,
      });
    };
  }, [clientX, firstClientX, firstClientY]);
```
