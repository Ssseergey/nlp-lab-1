# nlp-lab-1

### Критерии:
* Диаграмма графа есть в main.ipynb
* Все модели Pydantic
* Код LangChain в main.ipynb
* Запуск - последняя ячейка main.ipynb

### Установка

python 3.12.7

<code/>python -m venv venv</code>

activate venv

<code/>pip3 install -r requirements.txt </code>
<code/>pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu126 </code>

#### Запуск
* Поправить промпт на нужный в последней ячейке с кодом main.ipynb
* Нажать run all

### Краткое описание пайплайна

Пользователь вводит текст, в котором ожидается товар и страна, а также страна в которой он хочет найти аналог данного товара.

#### Planner
Нода, которая определяет параметры введенные пользователем

#### DescriptionWriter
Нода, которая пишет описание к товару, которые выбрал пользователь

#### ProductRecommender
Нода, которая рекомендует список похожих товаров в target_стране

### ProductCritic
Нода, которая выбирает из списка наиболее подходящий вариант


### Pydantic модели

class PlannerOutput(BaseModel):
*    item: str = Field(description="Product name")
*    country: str = Field(description="Country of origin")
*    target_country: str = Field(description="Target country for finding similar products")

class ProductDescriptionWriterOutput(BaseModel):
*    item_description: str = Field(description="Product description")

class ReActThoughtsOutput(BaseModel):
*    thoughts: str = Field(description="Current thoughts and reasoning")
*    action: str = Field(description="Next action: search_ddg, search_wikipedia, or finish")
*    action_input: str = Field(description="Input for the action")
*    target_items: List[str] = Field(description="Temporary target items list", default_factory=list)
*    evidence: List[str] = Field(description="Collected evidence from searches", default_factory=list)

class ProductRecommenderOutput(BaseModel):
*    target_items: List[str] = Field(description="Recommended products")
*    taste_similarity: List[float] = Field(description="Taste similarity score")
*    use_cases: List[str] = Field(description="Usage scenarios")
*    package: List[str] = Field(description="Packaging options")
*    search_evidence: List[str] = Field(description="Evidence from search results", default_factory=list)

class ProductCriticOutput(BaseModel):
*    target_item: str = Field(description="Selected product")
*    selection_reason: str = Field(description="Reason for selection based on search evidence")

class PipelineState(BaseModel):
*    human_request: Optional[str] = None
*    planner: Optional[PlannerOutput] = None
*    product_description_writer: Optional[ProductDescriptionWriterOutput] = None
*    product_recommender: Optional[ProductRecommenderOutput] = None
*    product_critic: Optional[ProductCriticOutput] = None