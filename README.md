            gameField[x][y] = 4;

        }
        else {
            gameField[x][y] = 2;

        }

    }


    private Color getColorByValue(int value) {
// 0, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048.
        switch (value) {
            case 2:
                return Color.PLUM;
            case 4:
                return Color.AQUA;
            case 8:
                return Color.GREEN;
            case 16:
                return Color.YELLOW;
            case 32:
                return Color.RED;
            case 64:
                return Color.PINK;
            case 128:
                return Color.PERU;
            case 256:
                return Color.PURPLE;
            case 512:
                return Color.GRAY;
            case 1024:
                return Color.BEIGE;
            case 2048:
                return Color.BLUE;

        }
        return Color.WHITESMOKE;
    }

        private void setCellColoredNumber(int x , int y, int value)
    {
        Color color = getColorByValue(value);
        if (value==0)
            setCellValueEx(x, y, color, "");
        else
            setCellValueEx(x, y, color, String.valueOf(value));
    }


    private void  drawScene ()
    {
        for (int i = 0; i < SIDE; i++) {
            for (int j = 0; j < SIDE; j++) {
                setCellColoredNumber(j, i, gameField[i][j]  );
            }
        }

    }


    //Движение
    private boolean compressRow(int[] row)
    {
        boolean result = false;
        for (int j = 0; j <SIDE;j++) {
            for (int i = 1; i < SIDE; i++) {
                //int temp = gameField[i-1];
                if (row[i - 1] == 0 && row[i] != 0) {
                    int temp = row[i - 1];
                    row[i - 1] = row[i];
                    row[i] = temp;
                    result = true;
                }
            }
        }
        return result;
    }

    private boolean mergeRow(int[] row)
    {
        boolean result = false;
            for (int i = 1; i < SIDE; i++) {
                if (row[i - 1] == row[i] &&row[i]!=0 ) {
                    row[i - 1] = row[i]*2;
                    score+=row[i-1];
                    setScore(score);
                    row[i] = 0;
                    result = true;
                }
            }
        return result;
    }

    @Override
    public void onKeyPress(Key key) {

        if (canUserMove()==false) {
            gameOver();
            return;
        }

        if (isGameStopped==true&&key!=Key.SPACE)
            return;

        switch (key) {
            case LEFT:
                moveLeft();
                drawScene();
                break;
            case UP:
                moveUp();
                drawScene();
                break;
            case DOWN:
                moveDown();
                drawScene();
                break;
            case RIGHT:
                moveRight();
                drawScene();
                break;
            case SPACE:
                isGameStopped=false;
                createGame();
                drawScene ();
                break;
        }

    }

    private void moveLeft() {
        boolean change = false;
        for (int i = 0; i < SIDE; i++) {
            if (compressRow(gameField[i])) change = true;
            if (mergeRow(gameField[i])) change = true;
            if (compressRow(gameField[i])) change = true;
        }
        if (change) createNewNumber();

    }

    private void moveRight() {
        rotateClockwise();
        rotateClockwise();
        moveLeft();
        rotateClockwise();
        rotateClockwise();
    }

    private void moveUp() {
        rotateClockwise();
        rotateClockwise();
        rotateClockwise();
        moveLeft();
        rotateClockwise();
    }

    private void moveDown() {
        rotateClockwise();
        moveLeft();
        rotateClockwise();
        rotateClockwise();
        rotateClockwise();
    }

    private void rotateClockwise() {
        int[][] result = new int[SIDE][SIDE];
        for (int i = 0; i < SIDE; i++) {
            for (int j = 0; j < SIDE; j++) {
                result[i][j] = gameField[SIDE - j - 1][i];
            }

        }
        gameField = result;
    }

    //вычисления
    private int getMaxTileValue()
    {
        int maxValue=0;
        for (int i = 0; i < SIDE; i++) {
            for (int j = 0; j < SIDE; j++) {
                if ( gameField[i][j] >maxValue)
                    maxValue=gameField[i][j];
            }
        }
        return  maxValue;
    }

    private boolean canUserMove()
    {
        boolean resault =false;
        //Проверка что есть 0
        for (int i = 0; i < SIDE; i++) {
        for (int j = 0; j < SIDE; j++) {
            if ( gameField[i][j] ==0) {
                return true;
            }
        }
    }
        //Проверка 2-х соседних
        for (int i = 0; i < SIDE; i++) {
            for (int j = 1; j < SIDE; j++) {
                if ( gameField[i][j] ==gameField[i][j-1])
                {
                    return true;
                }
            }
        }
        rotateClockwise();
        for (int i = 0; i < SIDE; i++) {
            for (int j = 1; j < SIDE; j++) {
                if ( gameField[i][j] ==gameField[i][j-1])
                {
                    return true;
                }
            }
        }
        rotateClockwise();
        rotateClockwise();
        rotateClockwise();

        return resault;
    }

    //удалить
    private void showRow(int[] row) {
        for (int i = 0; i < SIDE; i++)
            System.out.print(row[i]);
        System.out.println();
    }

}
