## Application Lifecycle Methods

```java
public class Main extends Application {
	@Override
	public void start(Stage primaryStage) {
		System.out.println("start()");
		try {
			VBox root = new VBox();
			Label lbl = new Label("Hello World");
			root.getChildren().add(lbl);
			Scene scene = new Scene(root,400,400);
			primaryStage.setScene(scene);
			primaryStage.setTitle("Hello World");
			primaryStage.show();
		} catch(Exception e) {
			e.printStackTrace();
		}
	}

	@Override
	public void init() throws Exception {
		// No GUI related operations allowed here
		System.out.println("init()");
	}

	@Override
	public void stop() throws Exception {
		System.out.println("stop()");
	}
	
	public static void main(String[] args) {
		launch(args);
	}
}

```

## Switching Scenes

http://www.javafxtutorials.com/tutorials/switching-to-different-screens-in-javafx-and-fxml/
