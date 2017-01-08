# crawlster
Generic crawler framework


## Sample usage

    class MyResultHandler(ResultHandler):
        fields = {
            "field_1": str,
            "field_2": str,
            "some_int_field": int,
            "boolean_field": bool,
            "float_field": float
        }
        def save_result(self, **values):
            my_database.insert(**values)
            my_database.commit()

    class MySimpleCrawler(Crawlster):
        start_steps = [
            ("http://example.com", "start_step"),
            ("http://somesimplesite.com", "start_step")
        ]
        thread_workers = 8
        result_handlers = [
            MyResultHandler
        ]
        
        def start_step(self, url):
            # do some stuff
            self.schedule(self.next_step, some_url)
            
        def next_step(self, url):
            # collect some results
            self.submit_result(field_1="hello", field_2="world",
                               some_inf_field=10, boolean_field=True,
                               float_field=10.22)
                               
    if __name__ == '__main__':
        crawler = MySimpleCrawler()
        crawler.start()
        