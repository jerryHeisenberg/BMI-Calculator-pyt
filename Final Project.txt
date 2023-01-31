//FINAL PROJECT
//MUSIC LIBRARY

#include<stdio.h>
#include<conio.h>

#define MAX_SONGS 1000

//It will represent a single music.
struct Song {
	char artist[50];
	char album [100];
	int year;
	char genre[20];
};

//Prototypes for function
void addSong(struct Song*library,int*size);
void deleteSong(struct Song*library,int*size);
void searchSong(struct Song* library, int* size);
void printLibrary(struct Song* library, int* size);



void main() {
	//Array to hold music Library
	struct Song library[MAX_SONGS];
	int size = 0, options;

	//Main Menu
	while (1) {
		printf("\n ((((((((((((((((((((((((((((((MUSIC LIBRARY))))))))))))))))))))))))))))))");
		printf("\n");
		printf("\n \t\t\t   1=> Add a Song");
		printf("\n \t\t\t   2=> Delete a Song");
		printf("\n \t\t\t   3=> Search for a Song");
		printf("\n \t\t\t   4=> Print Your music Library");
		printf("\n \t\t\t   5=> Quit");
		printf("\n \t\t\t   Enter your choice:");

		scanf_s("%d", &options);

		//using switch for options
		switch (options) {
		case 1:
			addSong(library, &size);
			break;
		case 2:
			deleteSong(library, &size);
			break;
		case 3:
			searchSong(library, &size);
			break;
		case 4:
			printLibrary(library, &size);
			break;
		case 5:
			printf("\n Libray is now closed!!");
			printf("\n");
			return 0;
		default:
			printf("\n INVALD! :(, Try Again.");
			break;
		}

	}

	printf("\n");
	_getch();

}
//add new song
void addSong(struct Song *library, int *size){
	char option, again = 'y';
	do {
		if (*size == MAX_SONGS) {
			printf("\n ERROR!: Music Library is full...");
			return;
		}
		printf("\n Enter artist:");
		scanf_s("%s", &library[*size].artist, sizeof(library[*size].artist));
		printf("\n Enter song:");
		scanf_s("%s", &library[*size].album, sizeof(library[*size].artist));
		printf("\n Enter year:");
		scanf_s("%d", &library[*size].year);
		printf("\n Enter genre:");
		scanf_s("%s", &library[*size].genre, sizeof(library[*size].artist));

		errno_t err;
		FILE* fp;
		err = fopen_s(&fp, "E:\\Hammad\\music.txt", "a");
		if (err == 0) {
			// file is opened successfully
			fprintf(fp, "\n Artist: %s,Song: %s,Year: %d,Genre: %s", library[*size].artist, library[*size].album, library[*size].year, library[*size].genre);
			fclose(fp);
		}
		else {
			// error occurred
			printf("Error opening file: %d", err);
			printf("\n!:(!:(!:(!:(!:(!:(!:(!:(");
		}

		(*size)++;
		printf("\n Song Added to the library.");
		printf("\n ((((((((((((((((((((((((((((((((((((()))))))))))))))))))))))))))))))))))))");
		
		printf("\n");
		printf("\n Wanna add another!? [y][n]: ");
		scanf_s("\n%c", &option);
		if (option == 'n') {
			again = 'y';
		}
		else if (option == 'y') {
			again = 'n';
		}

	} while (again != 'y');

}
//delete a song
void deleteSong(struct Song *library, int *size){
	int index; char option,again = 'y';
	do {
		if (*size == 0) {
			printf("ERROR!!: Music Library is empty.");
			printf("\n!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(!:(");
		}
		for (int i = 0; i < *size; i++) {
			printf("\n Artist: %s, Song: %s, Year: %d,Genre: %s", library[i].artist, library[i].album, library[i].year, library[i].genre);
		}
		printf("\n Enter the index of the song to delete: ");
		scanf_s("%d", &index);

		if (index < 0 || index >= *size) {
			printf("ERROR!!: invalid index.");
			printf("\n!:(!:(!:(!:(!:(!:(!:(!:(");
		}
		for (int i = index; i < *size - 1; i++) {
			library[i] = library[i + 1];
		}
		(*size)--;
		printf("\n Song is deleted from the Library successfully!!");
		printf("\n ((((((((((((((((((((((((((((((((((((()))))))))))))))))))))))))))))))))))))");

		printf("\n");
		printf("\n Wanna delete another!? [y][n]: ");
		scanf_s("\n%c", &option);
		if (option == 'n') {
			again = 'y';
		}
		else if (option == 'y') {
			again = 'n';
		}
	} while (again != 'y');
}
//to search song
void searchSong(struct Song *library, int *size) {
	int found = 0;
	char query[100],option,again='y';
	do {
		printf("\n Enter the name of the artist or song to search: ");
		scanf_s("%s", &query, sizeof(query));

		for (int i = 0; i < *size; i++) {
			if (strstr(library[i].artist, query) != NULL || strstr(library[i].album, query) != NULL) {
				printf("\n %d. %s - %s(%d)", i, library[i].artist, library[i].album, library[i].year);
				found = 5;
			}
		}
		printf("\n ((((((((((((((((((((((((((((((((((((()))))))))))))))))))))))))))))))))))))");
		if (!found) {
			printf("\n No songs found matching the search.");
			printf("\n ((((((((((((((((((((((((((((((((((((()))))))))))))))))))))))))))))))))))))");
		}

		printf("\n");
		printf("\n Wanna search again!? [y][n]: ");
		scanf_s("\n%c", &option);
		if (option == 'n') {
			again = 'y';
		}
		else if (option == 'y') {
			again = 'n';
		}
	} while (again != 'y');
}
//to print library
void printLibrary(struct Song *library, int *size) {

	errno_t err;
	FILE* fp;
	err = fopen_s(&fp, "E:\\Hammad\\music.txt", "r");
	if (err == 0) {
		// file is opened successfully
		fprintf(fp, "\n Artist: %s,Song: %s,Year: %d,Genre: %s", library[*size].artist, library[*size].album, library[*size].year, library[*size].genre);
		fclose(fp);
	}
	else {
		// error occurred

		printf("Error opening file: %d", err);
	}
	printf("\n");
	printf("\n Library has been written to music.txt");
	printf("\n ((((((((((((((((((((((((((((((((((((()))))))))))))))))))))))))))))))))))))");
}

