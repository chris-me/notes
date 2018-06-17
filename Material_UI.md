## Wrapping main content

```javascript
const MainWrapper = ({ classes }) => (
  <React.Fragment>
    <CssBaseline />
    <MainNavigation />
    <div className={classes.root}>
      <Route exact path="/" component={HomePage} />
      <Route exact path="/some" component={SomePage} />
      <Route exact path="/another" component={AnotherPage} />
    </div>
  </React.Fragment>
);

const styles = theme => ({
  root: {
    padding: 20,
    maxWidth: '960px',
    margin: '0 auto',
  },
});

```
