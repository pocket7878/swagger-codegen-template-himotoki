# swagger-codegen-template-himotoki

Swagger Codegen template for Swift4 + Himotoki. (under development, not ready for production)

## Difference between original

Original Swif4 template can be found [here](https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen/src/main/resources/swift4).

- Use [ikesyo/Himotoki](https://github.com/ikesyo/Himotoki) instead of Codable.
- Add extension `x-himotoki-transform-type` to specify Himotoki transformer.

**MODEL TEMPLATE ONLY**
```
-Dmodels
```

## Example

Swagger schema:

```yaml
HimotokiExampleModel:
    type: object
    properties:
    normal_double:
        type: number
        format: double
    data_str:
        type: string
        format: date-time
        x-himotoki-transform-type: Date
    url_str:
        type: string
        format: url
        x-himotoki-transform-type: URL
    required:
    - normal_double
```

Swift Model
```swift
import Foundation
import Himotoki

struct HimotokiExampleModel: Himotoki.Decodable {

    let normalDouble: Double
    let dataStr: Date?
    let urlStr: URL?

    public init(normalDouble: Double, dataStr: Date?, urlStr: URL?) {
        self.normalDouble = normalDouble
        self.dataStr = dataStr
        self.urlStr = urlStr
    }

    static func decode(_ e: Extractor) throws -> HimotokiExampleModel {
        return try HimotokiExampleModel(
            normalDouble: e <| "normal_double",
            dataStr: DateTransformer.apply(e <|? "data_str"),
            urlStr: URLTransformer.apply(e <|? "url_str")
        )
    }
}
```
