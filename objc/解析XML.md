`models.xml`


```xml
<?xml version="1.0" encoding="utf-8"?> 
<models>
  <model type="laptop"> MacBook Pro</model>
  <model type="laptop">MacBook</model>
  <model type="desktop"> iMac</model>
  <model type="desktop">Mac Mini</model>
</models>
```

```objective-c
NSString *path = [[NSBundle mainBundle] pathForResource:@"models" ofType:@"xml"];
path = [path stringByAddingPercentEscapesUsingEncoding: NSUTF8StringEncoding];

NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"file://%@", path]];
NSError *error;
NSXMLDocument *document = [[NSXMLDocument alloc]
         initWithContentsOfURL:url options:NSXMLDocumentTidyXML
         error:&error];
NSXMLElement *rootElement = [document rootElement]; 
if (rootElement != nil) {
	  NSArray *models = [rootElement elementsForName:@"model"]; 
  	if (models != nil) {
  		for (NSXMLElement *element in models) {
  				NSXMLNode *type = [element attributeForName:@"type"]; 
        	NSLog(@"Model: %@ Type: %@", [element stringValue], [type stringValue]);
      }
    } 
}
[document release];

```

